---
title: "[Android]GitHub  App"
datePublished: Thu Dec 21 2023 06:29:47 GMT+0000 (Coordinated Universal Time)
cuid: clqetozlj000a07jo3gvz24a3
slug: androidgithub-app

---

# Overview
This project aims to facilitate user searches by allowing them to input a username. Upon entering the desired username, the system displays a list of users containing or matching the input. Users can click on a specific entry in the list to view the repository information associated with that users in a web view.

## What I learned
- Retrofit
- GitHub Open API
- RecyclerView using ListAdpater
- Handler
- Recycler's item selecting animation

## Key Functions
To enhance the user experience and prevent exceeding the API call limit per minute, the following features were implemented:
- Debouncing applied to the EditText's addTextChangedListener using a Handler to prevent exceeding the API call limit per minute.
- When updating RecyclerView data with a scroll Listener, there was a issue of multiple API calls, which I resolved by implementing the isLoading status.
- Created Data Transfer Object(DTO) for efficient utilization of API data.

## Troubleshooting

#### RecyclerView Pagination Issue
Error: Page jumps occurred only during the second API call when loading the next page.  
Action: The page value was being sent to the API service as a query parameter, initialized to 0; however, it should have started from 1.  

##  GitHub link
%[https://github.com/LB-Brandon/GitHub_App]
