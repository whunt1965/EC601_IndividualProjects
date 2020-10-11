# EC601 Project 3
The primary objective of Project 3 is to deploy a complex open source system and execute a testing strategy to analyze features of this system. In my case, I will be focusing deploying and testing the WebRTC based conferencing application (Jitsi Meet)[https://jitsi.org/jitsi-meet/]. This project consists of two primary phases:
* Phase 1: Deploy an instance of Jitsi Meet and duplicate expected results (ie, produce a working instance of Jitsi Meet)
* Phase 2: Execute a testing strategy to identify potential vulnerabilities in Jitsi Meet (initial focus is on analyzing the signaling and finding security holes in session creation).

## Phase 1: Setting Up an Instance of Jitsi Meet
### Background Research and Set Up
Although a Jitsi Meet session can easily be created by visiting https://meet.jit.si, our goal was to set up our own instance of Jitsi Meet. To do so, I first looked at the (server download instructions)[https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-start] which allows you to host your own Jitsi instance on a Debian/Ubuntu system. Ultimately, I chose to implement an instance on an Amazon Web Services (AWS) EC2 virtual machine by following a (set of instructions)[https://aws.amazon.com/blogs/opensource/getting-started-with-jitsi-an-open-source-web-conferencing-solution/] available on the AWS open source blog.

Following these instructions, I created a virtual machine (with an Ubuntu OS) and installed Jitsi meet. During this process, it became apparent that it was necessary to link the instance to a legitimate domain name (which could be accessed through the browser) in order to ensure proper certificate validation (which is required by browsers such as Google Chrome in order to access a page). As such, I acquired a domain name and successfully launched a Jitsi meet instance available at that domain. 

### Analysis of Results
Once I successfully launched my Jitsi meet instance, I began testing its functionality to ensure it worked as expected.
**Observations**
* I tested accessing this instance via both computer and mobile (ios and Android devices) and was able to successfully host video conferences as well as utilize the chat, share YouTube videos, and other features. 
* After 2-3 test calls, my instance stopped responding. This may be a feature of the VM on which the instance is hosted, but will require further exploration during Phase 2.




