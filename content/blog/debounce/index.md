---
title: Debounce
date: "2024-06-27T22:40:32.169Z"
description: Understanding Debounce in Programming.
---

### Overview

The purpose of debounce is to make sure that when you type in a search box, the search function doesn’t get called too many times. It only triggers after you stop typing for a little while. This helps prevent unnecessary work, like sending too many requests to a server.

### Code Breakdown

#### 1. HTML Structure

```html
<input type="text" id="searchInput" placeholder="Search..." />
<div id="results"></div>
```
- **Input Field:** This is where you type your search query.
- **Results Div:** This is where search results could be displayed (though we’re just logging in this example).

#### 2. Debounce Function

```javascript
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        if (timeoutId) {
            clearTimeout(timeoutId);
        }
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}
```

- **Function Declaration:** `debounce` is a function that takes two inputs: `func` (the function to run) and `delay` (how long to wait after the last keystroke).
- **Timeout ID:** We create a variable called `timeoutId` to keep track of the waiting time.
- **Return Function:** This inner function is what will be called when you type. It:
  - Clears any previous timer (if you're still typing).
  - Sets a new timer that waits for the specified `delay` (e.g., 300 milliseconds).
  - Only calls the original function (`func`) once that time is up and you haven’t typed anything else.

#### 3. Simulated Search Function

```javascript
function performSearch(query) {
    console.log('Searching for:', query);
}
```
- **Search Function:** This function takes a `query` (what you typed) and logs it to the console. In a real app, it would look up results based on this query.

#### 4. Attaching the Event Listener

```javascript
const searchInput = document.getElementById('searchInput');

searchInput.addEventListener('input', debounce((event) => {
    performSearch(event.target.value);
}, 300));
```

- **Getting the Input Field:** We grab the search input element from the HTML so we can work with it in JavaScript.
- **Listening for Input:** We set up an event listener for when the user types in the search box (`input` event). 
- **Using Debounce:** We wrap our search function (`performSearch`) in the debounce function:
  - This means that every time you type, it checks if you’ve stopped typing for 300 milliseconds before actually calling `performSearch`.

### Summary

- **Purpose:** Prevents too many calls to the search function while you’re typing.
- **How It Works:** It waits for a short period after the last keystroke before doing anything.
- **Benefit:** Reduces unnecessary processing and improves performance, especially useful for web applications.
