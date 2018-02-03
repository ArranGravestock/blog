---
layout:     post
title:      React es6 Functions
date:       2018-02-03
summary:    New ways to create React functions with es6
categories: react es6 ecmascript functions
---

React has been ever changing and it makes it impossible to keep up with new features. Usually, nothing is decepreciated and if you're ever leaving your development environment you'll find out before it throws you some errors in your next package manager update.
 Es6 added arrow functions which can save you so much typing time when you create a new component in react. If you're not aware, to create a component in react you would do the following:
 ``` javascript
 class Item extends Component {
    constructor(props) {
        ...
    }
    
    render() {
        return (
            ...
        )
    }
}
```
It looks pretty, and you can it is somewhat clear how this works to the naked eye, your element is rendered with what ever you want and you pass the props to the element through using a constructor.

Es6 however, with the introduction of arrow functions makes it *really* pretty and saves you some time when you do not need to worry about states. The above code can be translated using arrow functions into this:
``` javascript
var Item = ({props}) => (
    ...your html element
)
```

Arrow functions have a wide range of uses, you can use them so that you can bind it to a component without having to define `this` in your constructor, meaning you can use partial application and closures. ES6 brings a lot of features to JavaScript that brings it in line with many other features that it has been lacking for years. If you're not working with ES6 or higher, you should really be considering it.