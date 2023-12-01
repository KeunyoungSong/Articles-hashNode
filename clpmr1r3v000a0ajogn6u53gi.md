---
title: "[DEBUG] MediaPlayer Error"
datePublished: Fri Dec 01 2023 14:58:11 GMT+0000 (Coordinated Universal Time)
cuid: clpmr1r3v000a0ajogn6u53gi
slug: debug-mediaplayer-error

---

## MediaPlayer 
#### error (-38, 0), error (1, -2147483648)
### Solution
-  MediaRecorder.start() called twice so I removed one.  

### Comment
I'm currently working on an app that utilizes MediaPlayer and MediaRecorder for voice recording. To make use of MediaPlayer, acquiring the necessary permissions is essential. I've been using a code snippet provided by Google Android to check the permission status.

Subsequently, I made the snippet into a function named requestPermission(), but that turned out to be a mistake. I inadvertently forgot that the startRecording method was already called within the requestPermission function, resulting in a double invocation—one inside the requestPermission and the other outside of it

Reflecting on this, I realize that the confusion stemmed from a poor choice of naming for the snippet. It would have been more prudent to name it in a way that clearly indicates the inclusion of both permission request and recording initiation. However, despite the naming issue, I am now finding ways to separate the functionality into two distinct functions.











