---
title: Pitivi GSOC Final Report
date: 2023-08-25 14:30:00 +/-0000
categories: [GSOC]
tags: [Pitivi, Open Source, GSOC]     # TAG names should always be lowercase
---

<img src="/assets/img/gsoc-final-report/gsoc_pitivi.png" alt="GSOC@Pitivi" width="600" height="300">

## **Introduction**
This blog is about my journey in GSOC'23 with Pitivi. I will be discussing about the project I worked on, the challenges I faced and the things I learnt during this journey. My journey to Open Source and GSOC is discussed in my previous blog [Starting with Open Source](https://jainl28patel.github.io/posts/OpenSourceJourney/)

## **Special Thanks**
I would like to thanks my mentors [Alexandru Balut(aleb)](https://gitlab.gnome.org/aleb) and [yatin](https://gitlab.gnome.org/yatinmaan) for their constant support and guidance throughout the project. Their constant support and guidance helped me a lot. They make my GSOC journey a lot easier, smoother and enjoyable.
I would also like to thanks Developers at GNOME for their help and support whenever I needed it. 

### A Short Description of the Goals of the Project
In the realm of open-source software, my engagement with the Google Summer of Code program brought me to a pivotal project: the migration of Pitivi's codebase from GTK3 to GTK4. Initially, porting might seem like a straightforward transition, yet as I embarked on this endeavor, it became evident that beneath the surface lay an array of challenges demanding thorough analysis, creative problem-solving, and a deep understanding of the software's architecture.

At first glance, porting Pitivi from GTK3 to GTK4 appeared to be a task of aligning a few syntax changes and making minor adjustments. However, as I delved into the intricacies of the project, I discovered that it was a profound undertaking that required extensive brainstorming, technical prowess, and a strategic approach.

#### Tracking my progress
All my work could be traced in this one MR [https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476)
<br>
The last commit that I made during my GSOC is [https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476/diffs?commit_id=1e9755e52ec2ca216b9eb757a875b051ea4fcbb1](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476/diffs?commit_id=1e9755e52ec2ca216b9eb757a875b051ea4fcbb1)


### The Path of Porting, Gradually Unveiled Complexity

The porting endeavor could be broadly classified into four distinct categories, each presenting its own set of challenges and requiring a unique approach:

1. **Class and Method Renaming** <br>
        Some portions of the code lent themselves to relatively straightforward solutions. This involved backporting techniques, renaming class methods and function    names, or identifying alternative methods to achieve the same functionality. The focus here was on optimization, simplicity, and efficiency.

2. **Guided by GTK's Migration Guide** <br>
        The GTK project's official migration guide provided a roadmap for handling certain issues. For these issues, the guide offered replacement ideas or alternative approaches that could be adopted to align with GTK4's specifications. Although not without their intricacies, these cases were well-documented and provided a valuable starting point.

3. **Delving into Code Structure** <br>
        The heart of the challenge lay in dealing with sections of code that resisted straightforward porting. This necessitated a deep dive into Pitivi's existing code structure, understanding the logic, and making substantial changes to accommodate the availability and unavailability of new APIs. One prominent example was the porting of the Drag and Drop feature, which demanded significant time and effort to ensure seamless functionality.

4. **Finishing Touches and Aesthetics** <br>
        As with any comprehensive migration project, there were those smaller, intricate finishing touches that might appear insignificant in terms of core functionality but contribute to the overall user experience. Addressing icon appearance, component placement, and sizing were among these subtle yet impactful tasks.

## **The Journey**

### Overcoming Challenges, One Step at a Time
The process of migrating the codebase required strategic planning and meticulous execution. It wasn't a journey without roadblocks. There were moments of frustration and times when seemingly insurmountable issues emerged. However, with each challenge, a sense of accomplishment emerged as solutions were devised through collaboration, research, and experimentation.


### Prioritization: Navigating Complex Issues during Code Migration
We prioritized the tasks that were needed to be able to launch Pitivi and do basic editing  . This approach allowed me to focus on critical issues and build a comprehensive understanding of the codebase, setting the foundation for tackling subsequent tasks.

### Addressing High-Priority Challenges: Laying the Foundation
I began by addressing critical issues preventing the app to be usable. The application couldn't even build or run properly due to numerous unported components. Some of the notable challenges I tackled were:

1. **Drag and Drop API Replacement** <br>
        One of the most demanding tasks was reworking the Drag and Drop functionality. This required extensive code changes as the API had undergone a complete overhaul. The migration involved adapting to event controllers, redefining data preparation, and implementing asynchronous drag and drop. Signals like drag-data-received and methods like gtk_drag_begin had to be replaced. This was crucial for seamless media, clip, and effect transfer between different parts of the application.

2. **Gtk Layout Replacement** <br>
        To ensure Pitivi's timeline, a key feature, appeared and functioned correctly, replacing the GtkLayout component was essential. This allowed the timeline to display as intended and support its full functionality.

3. **Adapting to Drawing Model Changes** <br>
        When we shifted Pitivi's code from GTK3 to GTK4, one of the biggest transformations happened in how things were drawn on the screen. In GTK3, widgets used a function called draw() to put their visuals on a canvas. But in GTK4, this changed. Widgets now create so-called GskRenderNodes to show their content. It was like switching from drawing on paper to using building blocks to create pictures. For us, it meant learning how to use these new blocks, understanding GtkSnapshot (a helper tool), and updating our code to fit this fresh way of making things look good on the screen. It's like learning a new art technique and applying it to our project.

4. **Integrating New gstGtkPlugins** <br>
        Ensuring proper media viewing in the viewer component posed a critical challenge. The viewer relied on Gtksink, a GTK3 component incompatible with GTK4. To overcome this, I incorporated new Gtk4-compatible plugins. This involved adding Rust plugins from GStreamer, building them, and integrating them into the application. Overcoming hurdles, including incorporating the plugins into the Flatpak manifest, was integral to this endeavor.


### Achieving Milestones: Building a Functional Framework
By prioritizing these high-impact challenges, I paved the way for the application to function more comprehensively. Resolving these issues addressed critical roadblocks that hindered Pitivi's migration to GTK4. The application evolved from an inaccessible state to a functional framework, setting the stage for the subsequent phases of functionality migration.


### Future Goals: Refining Functionality and Beyond
With the foundation laid and critical issues resolved, the subsequent phase involved migrating the functionality of each Pitivi component. This encompassed critical aspects such as the timeline, viewer, media library, effect library, and clip properties. These components collectively form the core of Pitivi's capabilities, and their successful migration would mark a significant stride towards realizing the project's overarching goals.
<br>
In the chapters that follow, I will delve into the intricacies of refining functionality across Pitivi's components. This journey highlights the intricate dance between technical expertise, creative problem-solving, and collaborative efforts that define the world of open-source development. 

### Where We're At: Pitivi's Functional and Flourishing State
After tackling various challenges, Pitivi's migration journey has led us to a noteworthy achievement—our application is now fully functional, with all the essential features seamlessly incorporated. This milestone reflects the hard work and dedication we've poured into the process.
<br>
The existing features have been seamlessly adapted to work within the GTK4 environment. With the successful porting, we can now effortlessly create, manipulate, and merge video clips along the timeline, all while leveraging Pitivi's robust video editing capabilities. This transition has empowered us to apply effects, refine video details, and access the functionalities we were accustomed to, now seamlessly integrated into GTK4.
<br>
Users can now interact with media through docked and undocked viewer modes, enhancing their experience and workflow. Alongside, features like the interactive introduction and the crucial ability to save and render projects confirm the success of our migration, demonstrating how we've adapted to GTK4's framework.


## Current State

<div class="row">
  <div class="column"><h4>Editor</h4><br><img src="/assets/img/gsoc-final-report/after-editor.png" alt="editor"></div>
</div>

<div class="row">
  <div class="column"><h4>Import Clip</h4><br><img src="/assets/gif/gsoc-final-report/after-import.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Drop to Timeline</h4><br><img src="/assets/gif/gsoc-final-report/after-playingVideo.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Previewer</h4><br><img src="/assets/gif/gsoc-final-report/after-preview.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Drag and Drop</h4><br><img src="/assets/gif/gsoc-final-report/after-DND.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Effects</h4><br><img src="/assets/gif/gsoc-final-report/after-effects.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Transitions</h4><br><img src="/assets/gif/gsoc-final-report/after-transition.gif" alt="before-undocked-viewer"></div>
</div>

<div class="row">
  <div class="column"><h4>Interactive Intro</h4><br><img src="/assets/gif/gsoc-final-report/after-interactiveintro.gif" alt="before-undocked-viewer"></div>
</div>

## Remaining Tasks: What's on the Horizon
With the main features of Pitivi up and running, our focus now shifts to a few remaining tasks marked as #TODO in the code comments. These tasks fall into the category of low-priority issues that may not directly impact core functionality but do affect certain UI aspects. For instance, adjusting the default window sizing or refining how clips appear when dropped onto the timeline—these are primarily UI-related adjustments that, while not critical, contribute to a polished user experience.
<br>
Another task involves enhancing the interactive intro by incorporating pointers to keyboard shortcuts. While not a critical functionality, it's a detail that can enhance user understanding and interaction.
<br>
Additionally, we plan to update the existing unit tests for compatibility with GTK4. Unlike the intensive brainstorming and code adjustments we faced earlier, this task is relatively straightforward. The groundwork for modifying the tests has already been laid during the code porting phase, so it's a matter of translating this knowledge into practical implementation. While less complex, ensuring the unit tests align with GTK4 remains a significant step in the migration process.
<br>
In summary, as we approach the finish line, our focus is on addressing these remaining tasks—primarily UI-related tweaks and ensuring unit tests are compatible with GTK4. Each task contributes to refining the application's usability and robustness, ensuring that the transition to GTK4 is comprehensive and seamless.

All the code written till now is present in [https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/476) and it will be merged after intensive testing is done and each and every piece of code is ported.

## Challenges and What I Learned: A Tough but Rewarding Experience
Migrating Pitivi's code from GTK3 to GTK4 was no walk in the park. It's probably one of the hardest things I've done in software development. First off, getting a grip on the entire codebase was like solving a big puzzle—challenging, to say the least. But what made it even trickier was writing code based on hunches when the app wasn't even working properly.
<br>
There were times when the usual fixes just didn't fit the bill because GTK4 didn't have the exact replacements for some things. So, I had to get creative and reimagine how things should work. On top of that, the lack of resources for GTK4 made things extra tough. Imagine having only documentation to rely on and no handy examples or guides.
<br>
But you know what? Through all the ups and downs, I learned a ton. I got better at reading API docs and figuring out how stuff works based on that. It taught me to trust my instincts, adapt on the fly, and not give up even when things got hairy. This experience proved that even in the wild world of coding, you can find your way with determination, creative thinking, and a bit of stubbornness.
