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
I am currently developing an app that involves the use of MediaPlayer and MediaRecorder for voice recording. To utilize the MediaPlayer, obtaining the necessary permissions is crucial. I found a code snippet on Google Android that checks the permission status. In that snippet, there were two instances where I needed to call a method to start recording when the permission is either granted or already granted.

Subsequently, I encapsulated the snippet into a function named requestPermission(), but that turned out to be a mistake. I inadvertently forgot that the startRecording method was already called within the requestPermission function, resulting in a double invocation—one inside the requestPermission and the other outside of it.

Reflecting on this, I realize that the confusion stemmed from a poor choice of naming for the snippet. It would have been more prudent to name it in a way that clearly indicates the inclusion of both permission request and recording initiation. However, despite the naming issue, I am now exploring ways to separate the functionality into two distinct functions.






