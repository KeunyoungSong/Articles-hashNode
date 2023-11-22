---
title: "Mbti_app"
datePublished: Wed Nov 22 2023 07:17:16 GMT+0000 (Coordinated Universal Time)
cuid: clp9fmcj0000f09l23dhv4pff
slug: mbtiapp
tags: android, kotlin, iuctouwsoy6oa

---

## What I learned
- How to use ViewPagerAdapter2
- Initializing Fragments while considering the Fragment lifecycle
    - Passing item (page) information using fragment.arguments
- Handling duplicate View (UI) initialization when iterating through a list containing View information
- Centralized data management for fragments within an Activity
- Implementation of the ZoomOutPageTransformer animation

## How it works
1. Navigate from MainActivity to TestActivity using Intent.
2. TestActivity contains a ViewPager2 object and a moveToNextQuestion() method to control it.
3. Apply the adapter in TestActivity, passing the instance of TestActivity to ViewPagerAdapter during this process.
4. In ViewPagerAdapter, reference TestActivity's member variables, methods, and layout objects using the received context.
    1. Note that ViewPagerAdapter receives the Activity's instance as FragmentActivity, so a type-cast to TestActivity is required to access the Activity instance's members. Otherwise, only FragmentActivity's members can be accessed.
5. ViewPagerAdapter, using TestActivity's context, implements the slide function between fragments created by itself.
6. ViewPagerAdapter has members from QuestionFragment to create and return fragments.
7. In QuestionFragment, configure the UI, use `(activity as TestActivity)` to store test results data in TestActivity, and call `TestActivity.moveToNextQuestion()` to move to the next page.
    1. There is logic in `TestActivity.moveToNextQuestion()` to navigate to the result Activity when reaching the last page.
8. Check the result page, and if the retryButton is clicked, move to MainActivity with the option `Intent.FLAG_ACTIVITY_CLEAR_TASK or Intent.FLAG_ACTIVITY_NEW_TASK`.

## Something I’m not sure about
- When wanting control over a View component, which approach is preferable? By lazy or lateinit?
- Is managing all Fragment data within an Activity the best approach?

## What could be better
- Managing UI data using a data class instead of individual processing would enhance maintainability.    

##  GitHub link
%[https://github.com/LB-Brandon/MBTI_APP/tree/main]
