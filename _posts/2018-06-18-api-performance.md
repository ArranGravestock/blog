---
layout:     post
title:      Performance Testing Call Methods (Whatwg-fetch vs xhr vs $.getJSON)
date:       2018-06-18
summary:    Time testing commonly used call methods in front-end development
categories: whatwg fetch xhr jquery getJSON get api call request receieve send
---

## Whatwg-fetch
### average - 18.58974202, minimum - 13.14501953, maximum - 32.78198242
```javascript
console.time();
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(json => console.timeEnd())
```

## JQuery getJSON
### average - 19.89865112, minimum - 15.05395508, maximum - 47.62817383
```javascript
console.time();
$.getJSON("https://jsonplaceholder.typicode.com/posts/1")
.done(function() {console.timeEnd()})

```

## XML HTTP Request
### average - 22.8302002, minimum - 16.55004883, maximum - 36.60620117
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

## Limitations
* Accessing the server's resources will most likely use a priority queue - anyone accessing the same resource as the time of testing will slow down the response,
* Console.time may start before the request is actually sent, as the browser may hold on the request before sending,
* Low amount of testing cases, limited to 25 results for each test.

## Takeaways
Comparing the results to the resource testing on jsperf, my testing through browser in Chrome follows suit with their results. Whatwg-fetch is slightly faster on average which can be detrimental to huge data resource requests - however the main performance restriction will be on the server (how data is retrieved from a database or cloud, and if it is efficient). The difference between each case is almost neglegible - there are other factors that contribute to which one to use (current codebase, preference, simplicity etc...). 

## Resources
- [Compiled test results, google sheets](https://docs.google.com/spreadsheets/d/1Y0j1OzyFhx8prrAdIH86dcwfnuaUEfl9CDVJ0127UHs/edit?usp=sharing)
- [Performance testing through 1000 tries, jsperf](https://jsperf.com/xhr-vs-jquery-ajax-vs-get-vs-fetch)
