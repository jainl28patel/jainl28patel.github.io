---
title: Symmetric Cryptography CryptoHack Writeup
date: 2023-09-26 09:00:00 +/-0000
categories: [CryptoHack]
tags: [Cryptography, CryptoHack]     # TAG names should always be lowercase
---

## Keyed Permutations
`bijection` is the mathematical term for a one-to-one correspondence.

## Resisting Bruteforce
`Biclique attack` is the best single-key attack against AES.

## Structure of AES
This challenge is pretty simple matrix operation. Just convert the byte value into chars corresponding to ASCII value.

```python
def matrix2bytes(matrix):
    """ Converts a 4x4 matrix into a 16-byte array.  """
    arr = [chr(i) for j in matrix for i in j]
    flag = ""
    for i in arr:
        flag += i
    return flag
```

## Round Keys
To solve this challenge just take XOR of each `s[i][j]` with `k[i][j]` and then convert matrix2bytes.

```python
def add_round_key(s, k):
    return matrix2bytes([list(s[i][j]^k[i][j] for j in range(4)) for i in range(4)])
```

## Confusion through Substitution
Take the value of `s[i][j]` as the index of `inv_s_box` and fill corresponding values followed by converting matix2bytes.

```python
def sub_bytes(s, sbox=s_box):
    return matrix2bytes([list(int(sbox[s[j][i]]) for i in range(4)) for j in range(4)])
```

## Diffusion through Permutation
Implement `inv_shift_rows`, take the state, run `inv_mix_columns` on it, then `inv_shift_rows`, convert to bytes and you will have your flag.

```python
def inv_shift_rows(s):
    s[0][1], s[1][1], s[2][1], s[3][1] = s[3][1], s[0][1], s[1][1], s[2][1]
    s[0][2], s[1][2], s[2][2], s[3][2] = s[2][2], s[3][2], s[0][2], s[1][2]
    s[0][3], s[1][3], s[2][3], s[3][3] = s[1][3], s[2][3], s[3][3], s[0][3]
    return s
.
.
.
inv_mix_columns(state)
inv_shift_rows(state)
print(matrix2bytes(state))
```

## Bringing It All Together

Conbine all the functions made till now and execute in reverse order of AES execution.

```python
def decrypt(key, ciphertext):
    # start from the last round key
    round_keys = expand_key(key)
    # Convert ciphertext to state matrix
    state = bytes2matrix(ciphertext)
    # Initial add round key step
    add_round_key(state, round_keys[N_ROUNDS])
    for i in range(N_ROUNDS - 1, 0, -1):
        inv_shift_rows(state)
        sub_bytes(state, inv_s_box)
        add_round_key(state, round_keys[i])
        inv_mix_columns(state)

    # Run final round (skips the InvMixColumns step)
    inv_shift_rows(state)
    sub_bytes(state, inv_s_box)
    add_round_key(state, round_keys[0])
    # Convert state matrix to plaintext
    plaintext = matrix2bytes(state)
    return plaintext

print(decrypt(key, ciphertext))
```

## Modes of Operation Starter
The provided `encrypt_flag()` function encrypts the `FLAG` using the `KEY` and the `decrypt(ciphertext)` function decrypts the same using same key. Just get the encrypted flag put it into decrypt flag function and convert hex `plaintext` into ASCII.

## Passwords as Keys
In this challenge we can get the encrypted flag using `encrypt_flag()` function. The question is how to get the `password_hash` because it makes the key form the `md5 hash` of a random word selected from dictionary of password given at `https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words`. 

This type of challenge could be easiliy solved using `bruteforce attack` as we have limited set of possibility of word that will make the `KEY`. We can simply try to decrypt using all keys made using each word and check for flag.

* Download the dictionary using `wget https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words`.

```python
from Crypto.Cipher import AES
import hashlib
import random


# /usr/share/dict/words from
# https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words
with open("words") as f:
    words = [w.strip() for w in f.readlines()]

# KEY = hashlib.md5(keyword.encode()).digest()
FLAG = "c92b7734070205bdf6c0087a751466ec13ae15e6f1bcdd3f3a535ec0f4bbae66"   # Encrypted FLAG => to decrypt


def decrypt(ciphertext, password_hash):
    ciphertext = bytes.fromhex(ciphertext)
    key = bytes.fromhex(password_hash)

    cipher = AES.new(key, AES.MODE_ECB)
    try:
        decrypted = cipher.decrypt(ciphertext)
    except ValueError as e:
        return {"error": str(e)}

    return {"plaintext": decrypted.hex()}
```

* Script to bruteforce attack
```python
import codecs
f = ""
for word in words:
    passHash = hashlib.md5(word.encode()).hexdigest()
    dec = decrypt(FLAG, passHash)
    try:
        f = codecs.decode(dec['plaintext'],'hex').decode('ascii')
        print(f)
        break
    except:
        continue
```

## ECB Oracle

ECB is the most simple mode, with each plaintext block encrypted entirely independently. In ECB mode, identical blocks of plaintext are encrypted to identical blocks of ciphertext. So we have to look for pattern in the blocks of ciphertext. To solve this we can think of one example.

* Suppose the first 16 bytes are `0` and if the first 17 bytes are `0` in both cases the AES Encryption of first block will always be same as it contains 16 `0`s. 
* If the flag starts with `crypto{` then if I pad with 15 `000000000000000`s or with 15 `000000000000000c` in both the cases the first block will be `000000000000000c` and will have same encryption for the starting block. 
* We just bruteforced the first char of the flag. We can do this for whole flag to find the flag.

For complete solution of the program you can refer to this [link](https://github.com/onealmond/hacking-lab/blob/master/cryptohack/ecb-oracle/ecb_oracle.py).


## ECB CBC WTF

In this challenge encryption is done using `AES-128-CBC` mode. In this mode the first block is XORed with the IV and then encrypted. The next block is XORed with the previous block and then encrypted. For decryption they use `AES-128-ECB` mode.

Assume a block of plaintext `pi` each of size `16` and `IV` as the initialisation vector. The first block of ciphertext `ci` is generated by `ci = AES(pi ^ IV)`. The second block of ciphertext `ci+1` is generated by `ci+1 = AES(pi+1 ^ ci)`. So we can say that `pi+1 = AES-1(ci+1) ^ ci`. So to decrypt the flag we can simply decrypt the ciphertext using `AES-128-ECB` and then XOR the ith decrypted block with the (i-1)th encrypted block. Refer to the following code for better understanding.

```python
# ECB CBC WTF
from Crypto.Cipher import AES
from pwn import xor
import requests

def encrypt():
    url = "http://aes.cryptohack.org//ecbcbcwtf/encrypt_flag/"
    response = requests.get(url)
    return response.json()['ciphertext']

flag = encrypt()
f = [flag[i:i+32] for i in [0,32,64]]
vi = f[0:(len(f)-1)]
f = f[1:]
def decrypt(data):
    url = "http://aes.cryptohack.org/ecbcbcwtf/decrypt/"
    response = requests.get(url + data + '/')
    return response.json()['plaintext']

for i in range(len(f)):
    f[i] = decrypt(f[i])

for i in range(len(f)):
    f[i] = xor(bytes.fromhex(f[i]),bytes.fromhex(vi[i]))

flag = ""
for i in f:
    flag += i.decode()

print(flag)
```


## Flipping Cookie
For complete solution of the program you can refer to this [link](https://github.com/onealmond/hacking-lab/blob/master/cryptohack/flipping-cookie/writeup.md)


## SYMMETRY
For this challenge we have been given `encrypt` and `encrypt_flag` function both of which uses OFB(Output feedback) mode of encryption. The first 16 bytes of the return of `encrypt_flag` is IV(initialisation vector). The `encrypt` function take the plaintext and gives the ciphertext. As one can see let `k = AES(KEY)` then `ciphertext = plaintext ^ k`. So we can say that `plaintext = ciphertext ^ k`. So we can get the key by XORing the first 16 bytes of the return of `encrypt_flag` with the ciphertext by providing the ciphertext as the plaintext to the `encrypt` function.


## Bean Counter
It is a simple `XOR` problem if you look at the function `increment` and the format of a `.png` file. `self.stup` will be `false` here so will always take else branch also the lines following it will give output which is same as `self.value`. Which means we are just taking the picture and XORing it with the `self.value`. Also as per the format of `png` file the `A PNG file is composed of an 8-byte signature header, followed by any number of chunks that contain control data / metadata / image data` which is already known as the name of file is given in the question. So we can just XOR the first 8 bytes of the image with the first 8 bytes of the output and we will get the flag.

```python
import requests

def fetch_encrypted_data():
    url = "http://aes.cryptohack.org/bean_counter/encrypt/"
    response = requests.get(url)
    return response.json()['encrypted']

def xor_bytes(byte_array1, byte_array2):
    return bytes(x ^ y for x, y in zip(byte_array1, byte_array2))

def main():
    png_header = bytes([0x89, 0x50, 0x4e, 0x47, 0x0d, 0x0a, 0x1a, 0x0a, 0x00, 0x00, 0x00, 0x0d, 0x49, 0x48, 0x44, 0x52])
    encrypted_data = bytes.fromhex(fetch_encrypted_data())

    keystream = xor_bytes(png_header, encrypted_data[:len(png_header)])

    decrypted_data = xor_bytes(encrypted_data, keystream * (len(encrypted_data) // len(keystream)))

    with open('bean_counter.png', 'wb') as file:
        file.write(decrypted_data)

if __name__ == "__main__":
    main()
```