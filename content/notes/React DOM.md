---
date: 06/03/2023
---


## What is it DOM?
DOM stands for *Documents Object Model* and in simple words is the structural representation of the UI of a web app in a tree form with all its components associated as branches of the tree. 
Every bit of html tag that you create is added to this tree allowing for the JavaScript to access it and modify their attributes, styles and other properties. 
This allows for JavaScript ot quickly alter a website UI. 

## What is React DOM?
React introduces the concept of Virtual DOM, which is a lightweight replica of the real DOM kept by React. 
DOM manipulation takes time, but React DOM manipulation is very lightweight allowing for quick manipulation.
Whenever the state of any component is updated, the virtual DOM is reconstructed. 