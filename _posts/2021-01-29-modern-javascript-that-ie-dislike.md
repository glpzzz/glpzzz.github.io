---
title: "Modern JavaScript that Internet Explorer 11 dislikes"
date: 2021-01-29
description: "Things I have discovered when trying to run JavaScript code on IE11"
image: https://apimania.netlify.app/api/txt2img?text=Modern%20JavaScript%20that%20Internet%20Explorer%2011%20dislikes&font=Fira%20Code&format=4:3
---
Yesterday I was call to fix some issues on a website I "fixed" some weeks ago.
Main thing was to update the theme (Wordpress). They had a lot of old stuff, 3rd
party libraries and the sort of stuff that you can expect of something written
on 2017. 

The biggest change was migrating forms validation to checking values on change,
and before submit to the native HTML5 validation techniques.

And as part of the job I started to write the JavaScript code using the native
features instead of jQuery or other "tricks".

Now, I have to fix some stuff again because on 2021 there is some people still
using Internet Explorer 11 (IE11).

Things that does not work:

- Arrow functions 
  - changed to old `function()` functions
- `{variable}` interpolation
  - changed to old `Array.prototype.replace()`
- native `Array.prototype.forEach()`
  - changed to `$.each()` from jQuery (good in the context)
- native `String.prototype.replaceAll()`
  - changed to `String.prototype.replace(RegExp, string)`

The list is in construction...

PS: Thanks to my colleagues `epgeroy` and `geekmidget` for the remote debugging as I don't have IE11 on my Ubuntu machine.
