# EC601 Project 3
The primary objective of Project 3 is to deploy a complex open source system and execute a testing strategy to analyze features of this system. In my case, I will be focusing deploying and testing the WebRTC based conferencing application [Jitsi Meet](https://jitsi.org/jitsi-meet/). This project consists of two primary phases:
* Phase 1: Deploy an instance of Jitsi Meet and duplicate expected results (ie, produce a working instance of Jitsi Meet)
* Phase 2: Execute a testing strategy to identify potential vulnerabilities in Jitsi Meet and other issues in the application.

## Phase 1: Setting Up an Instance of Jitsi Meet
### Background Research and Set Up
Although a Jitsi Meet session can easily be created by visiting https://meet.jit.si, our goal was to set up our own instance of Jitsi Meet. To do so, I first looked at the [server download instructions](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-start) which allows you to host your own Jitsi instance on a Debian/Ubuntu system. First, I chose to implement an instance on an Amazon Web Services (AWS) EC2 virtual machine by following a [set of instructions](https://aws.amazon.com/blogs/opensource/getting-started-with-jitsi-an-open-source-web-conferencing-solution/) available on the AWS open source blog, but then later switched the deployment to the Google Cloud Platform. 

Following these instructions, I created a virtual machine (with an Ubuntu OS) and installed Jitsi meet. During this process, it became apparent that it was necessary to link the instance to a legitimate domain name (which could be accessed through the browser) in order to ensure proper certificate validation (which is required by browsers such as Google Chrome in order to access a page). As such, I acquired a domain name and successfully launched a Jitsi meet instance available at that domain. 

![image of login](https://github.com/whunt1965/EC601_Project3/blob/main/Snip20201011_13.png)

### Analysis of Results
Once I successfully launched my Jitsi meet instance, I began testing its functionality to ensure it worked as expected.


**Observations**
* I tested accessing the instance deployed on AWS via both computer and mobile (ios and Android devices) and was able to successfully host video conferences as well as utilize the chat, share YouTube videos, and other features. 
* After 2-3 test calls, my instance stopped responding. This may be a feature of the VM on which the instance is hosted, but will require further exploration during Phase 2.
* I then relaunched the instance on GCP and again was able to successfully run the instance on both computer and mobile devicves. Notably, though, accessing the instance via Safari (rather than via Chrome) led to camera streams being disabled (which may be a result of how WebRTC is implemented in the Safari browser).

## Phase 2: Testing Vulnerabilities and other Application issues
In phase 2, I began testing Jitsi for potential vulnerabilities and other issues in the application. 

### Testing Vulnerabilities 
#### Session Disruption
One potential issue that became immediately obvious is that a user may simply append "/(room name)" to the host domain and access any meeting with that that room name. This could allow an attacker to join a call without the permission of the other users on the call. The easiest remediation for this issue (which is readily available within a Jitsi session's security settings) is to add password protection to any meeting (or, at a minimum, use a waiting room feature). Thus, this vulnerability can be easily mitigated.

#### Incomplete Session Termination on Mobile (when closing tab)
In testing accessing the instance via mobile, I noticed an issue where simply closing the tab on a mobile (phone/tablet) doesn't actually automatically disconnect the user from the session. While the media (audio/video) streams from the mobile are no longer available, the user is still "logged in" (for a short period of time on the scale of 1-2 minutes) and any information that user shared in the chat remains (and anyone who logs into the session during this window will have access to the chat history even after they are logged out). This could be very dangerous if sensitive information is shared in the chat. To remediate this, I would suggest again protecting all sessions with a password. Notably, this issue does not seem to occur on the [online Jitsi instance](https://meet.jit.si).
![image of login](https://github.com/whunt1965/EC601_Project3/blob/main/Snip20201012_15.png)

#### Password Issues
While users can add password protection to a session, this feature is not without flaws. First, I noticed that I was unable to add password protections to meetings through Safari. This may be some sort of compatibility issue with the browser, but effectively means that a user accessing the instance through Safari will be unable to add password protection to a meeting (note: this is not an issue in Google Chrome). In addition, while a user might password protect an ongoing session, the password protection is removed after the session ends. This means that a recurring session would only have password protection each time it's added when users begin that session. One potential vulnerability here would be that a recurring session with a large audience coudld be targeted by an attacker who logs into the session (provided they know the session id) before the password protection is added. 

#### Vulnerabilities within Chat
Jitsi has reported issues in the past of the [chat being susceptible to XSS attacks](https://community.jitsi.org/t/jitsi-users-xss-in-chat-window-of-meet-jit-si/7021), although these vulnerabilities appear to have since been patched. Nevertheless, I attempted to see if XSS atatcks were still possible by analyzing the chat source code and attempting to inject javascript into the chat window (which was ultimately unsucessful).  

#### Vulnerabilities within Youtube Sharing Feature
Jitsi has a functionality which allows users to share YouTube videos in a session. I thought this might be an interesting attack vector, and attempted to find vulnerabilities in this feature. First, I tried to inject JavaScript into the "Share YouTube Video" capability (ie, inputting alerts or attempting to append Javascript to the end of a legitimate URL) but was unsuccessful in having any of this code executed. Indeed, Jitsi appears to analyze the URL prior to executing, so simple JS inputs were ruled illegal and any JS appended to the end of legitimate video URL appears to have just been discarded. I also tried inputting non-YouTube video URL's into this share feature(eg, google.com and even Youtube.com), but the system correctly identified these as invalid inputs and rejected them accordingly. 

### Other Usability Tests
#### Overloading Session with Multiple Users
I wanted to test how many users my Jitsi instance could handle simultaneously and therefore open multiple connections (from my same computer, though identified by Jitsi as multiple users). After opening 13 connections, I noticed that Jitsi began to drop connections. Indeed, within 2 minutes, 5 of the connections had been lost. This issue could easily have been caused due to my browser/computer, but still offered insihght into how my instance my handle large sessions.

#### Large inputs in Chat Window
I tested to see how the instance would handle large inputs in the chat window by copying and pasting text from the same document (which itself was in excess of 5 pages of type text) multiple times into the chat window. Ultimately, the instance appeared to handle this well by limiting how much of text was posted to that chat each time I hit submit (indeed, it ultimately took several submissions for the entire text to be emplaced in chat, but it did not appear that any information was lost). 


