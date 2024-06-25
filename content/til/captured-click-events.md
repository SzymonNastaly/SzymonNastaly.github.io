+++
title = "Captured Click Events and Browser Extensions"
description = "How to (nearly) always handle clicks in the content script UI"
date = 2024-05-24
+++
In the chrome extension I am developing, I inject a content script UI into the page. This UI is a React interface that has a few buttons (with `onClick` handlers). 

I found that some sites prevent DOM elements lower in the tree to react to click events. They do this by registering their EventListener with `{capture: true}` and the `stopPropagation()` method. 

Unfortunately we cannot do anything against both of those things. 

Ideally we could just remove all of the event listeners completely. Unfortunately I was not able to do that: 

1. There are no built in browser functions to get the registered event listeners of some DOM element
2. I could not get existing npm packages to work in my browser extension.

What I therefore decided to do is to add listeners for the events: `click`, `mouseDown` and `mouseUp` , but to call the handler only once:

```js
import React, { useRef } from 'react';

const MyComponent = () => {
  const eventHandled = useRef(false);

  const handleEvent = (event) => {
    // Check if the event has already been handled
    if (eventHandled.current) return;

    // Mark the event as handled
    eventHandled.current = true;

    // Event handling logic here
    console.log('Event handled:', event.type);

    // Reset the handled flag after a delay
    setTimeout(() => {
      eventHandled.current = false;
    }, 300); // Adjust the delay as needed
  };

  return (
    <button
      onMouseDown={handleEvent}
      onMouseUp={handleEvent}
      onClick={handleEvent}
    >
      Click Me
    </button>
  );
};

export default MyComponent;
```

In the case that a website captures all three of those events, this will of course still not help. But in my testing I have only found websites that were capturing the click event. In that case, the MouseDown event will still be available for the browser extension.