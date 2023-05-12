---
title: Starting with Open Source
date: 2023-05-12 09:00:00 +/-0000
categories: [Experiences]
tags: [Pitivi, Open Source, GSOC]     # TAG names should always be lowercase
---

This post is about my journey to open source and my GSoC journey. This blog also marks the beginning of a skill that I wanted to pursue, which is blogging about my **experiences**, **technology**, and **computer science**.

## **The Beginning**

Just like an average Indian student after grinding for 2 consecutive years for competitive exams but this time with the hit of Covid-19 and after a tough selection process I finally got into one of my dream college, IIT Roorkee and with a good rank of AIR-309 I got my favourite branch **Computer Science** :) .

After getting into college I explored many fields including Competitive Programming, ML (Machine Learning), little bit DL (Deep Learning), Software Development etc my interest in my branch amplified a lot. Luckily I got into the topmost Software Development Club, [SDSLabs](https://sdslabs.co/) which highly promoted Open Source culture in our campus.

---

## **Getting into it**

![openSource.png](/assets/img/post_img/openSource.jpg)

After getting into contact with people really passionate about software development especially Open Source softwares (OSS), I got amazed to know about all the OSS that are being used everyday by billions of people and how it is impacting our life. The fact that some of the best softwares that I used were Open Source and I can also contribute in it blew off my mind. Some of the OSS that I had used till then were Ubuntu, VLC player, OBS studio, VSCode, Godot Engine and many more.

I got really interested into and thought to contribute in some of the OSS.

### **My first Pull Request**

By this time I had some experience in developing softwares using C++ and this was the first time I was going to contribute to a Open Source software. I started with [Rootex](https://github.com/sdslabs/rootex) which is a C++ 3D Game engine developed by some of my seniors in SDSLabs. I worked on a issue which focused on the optimizing the shader performance by shifting Normal map calculation to Vertex shader from Pixel shader.

### **Challenges and Learnings as a newbie**

It was not that tough to begin with, to contribute to any OSS you just need to setup the project as per given instructions, understand the development workflow which has been decided by the maintainers of that project and with a little or even zero idea just jump into it, you will learn thing on the go. 

My work was to optimize the shader but I never worked with computer graphics but this gave me an opportunity to learn. I took 10-15 day for that but in this time I learnt basics of computer graphics and understood my work after which it was preety easy to work. The main point is that I learnt a thing which I might have not thought to learn and along with learning I implemented that thing into a Big software.

### **Getting it merged**

After Implementing and testing on my side I made my first PR which resulted into some reviews from maintainer which I resolved and tadda!! it got merged. Now I made my first contribution into a really large piece of software which is really great. Though it took a long time but it taught me a lot, which in my opinion is the most important part.

---

## **Benefits of Contributing to Open Source**

From my limited amount of experience in contributing to 5-6 OSS one thing that I can say as a newbie is that. 
Its a well known fact that to learn new thing in depth and to build proper understanding of it one should try to implement it practically and should stay in touch with experienced person in that field. In the field of Software Development, Open Source provides you both. It provides you with a chance to work and implement your learnings into real world projects that actually makes impact and to remain in touch with a great community of developers around the world.

Ofcourse you can work on a small personal project or a larger one if you are experienced but generally the bar for that is too high. Its really difficult to build a software that big just for learning purpose rather it would be beneficial to work on the preexisting softwares in initial stage and that is were OSS comes in, it provides hands on experience on really good pieces of softwares, provides with necessary support and guideance from experienced peoples and great community to interact with.

---

## **Getting into GSOC**

![GSOC.png](/assets/img/post_img/GSOC.png)

After having some contributions in some of the OSS, in the 2nd year of my college I thought to take part in GSOC which is a kind of intership for beginners in which you get paid for working into some project in some Open Source software.

I started making contribution in Godot for a month but unfortunately it didn't took part in GSOC this year, so after mid term exams in around March after the announcement of accepted GSOC organisations I started to search for some organisation that interests me. While searching I came across [Pitivi](https://www.pitivi.org/) which is a video editor for Linux and designed to be the default video editing software for the [GNOME](https://www.gnome.org) desktop environment. I felt it to be a good organisation to work in so I joined it's developer's chat on Matrix and started looking around for `good first issues` to work upon. 

At that time I was using a `Mac M1` and I was not able to setup Pitivi on my Mac. So after talking to the maintainer I thought of picking up issue to port Pitivi on Mac OS as it is possible to do so. I worked on it for 2 weeks and then we came point where we can't proceed further until whole software was ported from GTK3 to GTK4 which was actually a work in progress and also a project Idea provided by the organisation for GSOC'23.

I thought about it and picked up that project, went through it and worked on it for a while to get a clear idea about my work and then comes the proposal writing period. I wrote the proposal, got it reviewed from the mentor and submitted. 

GSOC result came on `4th May 2023` and unfortunately I was not selected as someone else also submitted a proposal for the same project and was selected. Surprisingly on `7th May 2023` I got mail of selection as somehow that person was not qualified for GSOC for whatever reason. Mentor choose me as his replacement and finally after too many ups and downs I got selected to GSOC. All thanks to `Alexandru Băluț (aleb)` for helping me at every moment where I was stuck.


### **My work**

My main project is to complete the ongoing migration of Pitivi from GTK3 to GTK4 under the mentorship of `Alexandru Băluț (aleb)` and `yatin`. If I am able to complete it before deadline than my plan is to finish the porting of Pitivi to Mac OS which I picked up earlier.

#### **And the Journey Continues ...**