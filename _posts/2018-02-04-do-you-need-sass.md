---
layout:     post
title:      Do you really need a css preprocessor?
date:       2018-02-04
summary:    SASS? LESS? Is it a must in 2018?
categories: css styles sass less preprocessor
---

CSS preprocessor have provided some much needed functionality that classic css has been lacking for several years but recently css has some new features that puts the question whether or not you need one. Features that other languages that help provide a much more maintainable style sheet, less complexity and better seperation of concerns really drove much of the industry towards using them. Recently though, CSS has released some new functions that incorporate these features.
## Variables
``` css
:root {
    --variable-name;
}
    
.element {
    background-color: var(--variable-name);
}
```
It even supports inheritance, custom property fallback and you can even access the variable in JavaScript!
## Nesting
Unfortunately nesting isn't really possible like it is in SASS or LESS, which results in a messier code base than using SASS. Sure, you can use combinators but generally in SASS it is much cleaner, for example:
``` css
.base > .child > .grandChild {
    //properties
}
```
Is equivalent to in SASS: 
``` sass
.base {
    .child {
        .grandChild {
            //properties
        }
    }
}
```
Although it is longer, it is must clearer - specificity of the css is obviously the larger problem but it makes it much easier to modify in my opinion.

## Partials
Again, is not possible in CSS - it is simply a performance saving tool which helps to generate less CSS making it easier to create block generated pages creating a faster perceived loading time.

## Import
Both support import but SASS is slightly different and does not generate another HTTP request when importing css files. SASS builds on top of the current CSS `@import` and will take the file that you want to import and combine it with the file being imported. Both have the same syntax of `@import 'filelocation';`

## Mixins
It's useful to define a set of CSS declarations you want to rewrite several times, enabling you to have a code saving technique, similar to functions in any other language and including passing a variable to it making it very powerful in code management. CSS does not support variable passing which makes it slightly less viable, however still a very good feature in standard CSS.

## Inheritance & Extending
CSS can only inherit through their declarations, rather than inheriting a group of declarations, making it sloppy to change or edit code. SASS however, uses the `@extend` syntax to state the extension of a declaration. For example in SASS, extending a class of declarations is as follows:
``` sass
%button-shared {
    color: #333;
    padding: 1em;
}

.success {
    @extend %button-shared;
    border-color: green;
}

.error {
    @extend %button-shared;
    border-color: red;
}
```
The code above the elements `.success` and `.error` will behave the same as %message-shared, so anywhere it is called will also display `.success` and `.error`, saving you time on writing multiple class names on the HTML elements.

## Operators
Math is pretty useful especially when generating your widths, heights or padding of elements. You can use it without calling any extra functions in SASS, but the syntax in CSS is slightly different in that you have to call `Calc(your math here);`. They both support `+`, `-`, `*`, `/` and `%`. 

## Conclusion
Do you really need a precompiler? Well, it depends. Although SASS is useful in almost every way it's not really necessary for every project. What's the point of using a precompiler for just dressing up a landing page and that's it? You'd probably spend more time configuring your setup rather than editing your page. Also you will probably never the majority features SASS provides to benefit the project. Big projects where your elements are reoccuring over and over throughout your website? Sure, you probably want a precompiler. Maintaining your code base it a lot easier and you can speed up a lot of the process by ensuring you have a good structured code.

In the future, I doubt that you'll need a precompiler - CSS is slowly closing the gap and with [CSS4 Selectors](https://drafts.csswg.org/selectors-4/) you'll probably be on a level playing field when css3 update to css4 standards.
