---
title: Lecture 9 - Reactive Frameworks
layout: doc
---

# Lecture 9 Blog Post - Further Exploration with Vue

In this lecture, we learned about "reactive programming", and particularly started looking at the language Vue. In this blog post, I decided to try to apply some of the topics from lecture in my own mini Vue explorations using <a href="https://play.vuejs.org/#eNp9kUFLwzAUx7/KM5cqzBXR0+gGKgP1oKKCl1xG99ZlpklIXuag9Lv7krK5w9it7//7v/SXthP3zo23EcVEVKH2yhEEpOhm0qjWWU/QgccV9LDytoWCq4U00tTWBII2NDBN/LJ4Qq0tfFuvlxfFlTRVORzHB/FA2Dq9IOQJoFrfzLouL/d9VfKUU2VcJNhet3aJeioFcymgZFiVR/tiJCjw61eqGW+CNWzepX0pats6pdG/OVKsJ8UEMklswXa/LzkjH3G0z+s11j8n8k3YpUyKd48B/RalODBa+AZpwPPPV9zx8wGyfdTcPgM/MFgdk+NQe4hmydpHvWz7nL+/Ms1XmO8ITdhfKommZp/7UvA/eTxz9X/d2/Fd3pOmF/0fEx+nNQ==">Vue SFC Playground</a>.

## Project 1

For my first exploration, I made a simple form that lets you type in your name. When you press the submit button on the form, the hello message below it updates with your name. I did this using two reactive variables and binding the button's click to a function that sets the second variable to the first. Below is a picture of the project along with its code.

<div style="display: flex; align-items: center;">
    <img src="/../assets/images/vue1-img.png" alt="vue 1 image" style="width: 300px; margin: 10px;"/>
    <img src="/../assets/images/vue1-code.png" alt="vue 1 code" style="width: 300px; margin: 10px;"/>
</div>

## Project 2

For my next exploration, I made some text with two buttons below it that lets you change the text's size. This uses one reactive variable "value" and binds the buttons to functions that will change this value (increaseValue and decreaseValue). Finally I put the reactive variable into the style element of my text. Below a picture of the project and the project's code.

Project Image:
<img src="/../assets/images/vue2-img2.png" alt="vue 1 image"/>

Code:
<img src="/../assets/images/vue2-code.png" alt="vue code"/>

## Project 3

For my final exploration, I made a simple rock paper scissors game. The user types in either "rock", "paper", or "scissors" into the input. When they press submit, the computer chooses a random one of these to play, and displays who won. It includes reactive variables for the user's choice, the computer's choice, and the result. It also includes the game logic when the user presses submit. Below is an image of the project and its code.

<div style="display: flex; align-items: center;">
    <img src="/../assets/images/vue3-img.png" alt="vue 1 image" style="width: 300px; margin: 10px;"/>
    <img src="/../assets/images/vue3-code.png" alt="vue 1 code" style="width: 300px; margin: 10px;"/>
</div>
