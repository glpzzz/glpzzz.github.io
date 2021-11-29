---
title: "How to toggle attribute values using jQuery"
date: 2020-10-10
description: "Some ways to toggle the value of an attribute between two options by using jQuery. Go to the last one if you are in a hurry!"
image: https://glpzzz.is-a.dev/assets/toggle-attribute-value-jquery.png
---
I just came across a very simple [question on StackOverflow](https://stackoverflow.com/questions/64290056/how-to-use-button-action-to-toggle-between-two-states-of-a-textarea-using-jquery). In the end is how to toggle the value of an attribute using jQuery.

## The long version

This is the explanatory version I created when the author of the question requested for an explanation of my original code. Most of the comments are obvious.

```javascript
$('thebutton').click(function(){
   var currentRows = $('thetextarea').attr('rows'); //obtain the current number of rows in the textarea
   var newRows; //declare a variable to hold the new number of rows
   if(rows == 1){ //if just one...
       newRows = 5; // it should become 5
   }else{ 
       newRows = 1; //else, (not 1), become 1
   }   
   $('thetextarea').attr('rows', newRows); //assign the new value to the rows attribute of the textarea
});
```

## Short version

My original answer was...

```javascript
//short version, the one in the answer
$('thebutton').click(function(){
   $('thetextarea').attr('rows',  $('thetextarea').attr('rows')==1?5:1);
});
```

I've created a [gist](https://gist.github.com/glpzzz/762afe32e8c245216869e9b1bd0aaac0) for this two versions.

## The fancy version (new for me)

I still had the doubt if this could be done easier, and find the next way thanks to this [answer](https://stackoverflow.com/questions/18665031/how-to-toggle-attr-in-jquery/18665152#18665152):

```javascript
$('thebutton').click(function(){
   $('thetextarea').attr('rows',  function(index, attr){
       return attr == 1 ? 5 : 1;
   });
});
```

Just learnt a new thing today. I can sleep in peace.
