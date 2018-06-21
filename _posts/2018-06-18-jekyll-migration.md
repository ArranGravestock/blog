---
layout:     post
title:      Performance Testing Call Methods (Whatwg-fetch vs xhr vs $.getJSON)
date:       2018-06-18
summary:    Time testing commonly used call methods in front-end development
categories: whatwg fetch xhr jquery getJSON get api call request receieve send
---

ping average is 5.2ms for jsonplaceholder


```javascript
console.time();
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(json => console.timeEnd())
```

```javascript
console.time();
$.getJSON("https://jsonplaceholder.typicode.com/posts/1")
.done(function() {console.timeEnd()})

```

```javascript
console.time()
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
	  console.timeEnd()
  }
}
xhr.open("GET", "https://jsonplaceholder.typicode.com/posts/1", true);
xhr.send();
```
