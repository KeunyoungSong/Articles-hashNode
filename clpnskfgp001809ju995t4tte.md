---
title: "MediaPlayer App"
datePublished: Sat Dec 02 2023 08:28:28 GMT+0000 (Coordinated Universal Time)
cuid: clpnskfgp001809ju995t4tte
slug: androidmediaplayer-app

---

# Overview
This application enables users to record their voice and play it back using MediaRecorder and MediaPlayer functionalities. Users can visually inspect the amplitude of the recorded voice on a canvas within a CustomView, utilizing the amplitudes provided by MediaRecorder. This implementation employs Handler and runnable to transmit amplitude data to the CustomView.

## What I learned
- MediaRecorder
- MediaPlayer
- Permission Request
- Canvas
- Handler

## Key Functions
- Record your voice
- Play your voice
- Visualize voice amplitude

## Troubleshooting
#### Issue: `MediaRecorder` is deprecated
- Action: Branching based on the device SDK version
- Explanation: A new version of MediaRecorder was introduced, requiring context and a higher API level of 32. Consequently, a conditional statement was employed `(Build.VERSION.SDK_INT >= Build.VERSION_CODES.S)` to use the new function for API levels 32 and above, and the deprecated function for lower versions.  
**Notice!** The `RECORD_AUDIO` permission is considered 'dangerous' due to potential risks to user privacy. Apps using dangerous permissions must request user approval at runtime, starting from Android 6.0(API level 23).  

## Something I’m not sure about
- I obtained a code snippet from Google for requesting permissions, which required placing the function to execute when permission is granted in two different places. : one inside a 'when' statement and the other in a separate variable called `requestPermissionLauncher`. I faced difficulty in naming this function.  
Why can't I simply request permission, receive a result to check if permission was granted successfully, and then proceed to other logic? While I understand my approach may not be the best practice, I couldn't find an alternative.

## What could be better
- Find alternative methods for implementing permission requests
- Use more descriptive names for functions to enhance clarity regarding their functionality

##  GitHub link
%[https://github.com/LB-Brandon/Recorder_APP]
