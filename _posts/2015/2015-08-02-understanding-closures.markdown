---
layout: post
title: "Understanding Closures"
date: "2015-9-2 09:12"
---
Like most things in Javascript, closures can appear more difficult to grok than they actually are.  At its root, a closure is a function within another function. Lets demonstrate this with code.

```
function foo(name) {
    var greeting = "sup";

    return function bar(place){
       console.log(name + " says " + greeting + " " + place);
    };
}

```

Voila! That's a closure. But what makes a closure so special, let alone one of Javascript's most powerful features? The magic lies in the access afforded to the inner bar function.  Not only does the inner function bar have access to its own scope chain (all the variables within its curly bracket), but it also has access to both outer function foo and the global's scope chain.  Whenever you use variables outside your immediate lexical scope you are creating closures.

Again, you may be asking yourself "why is this even important?" What makes closures interesting is that you can use the variables of your outer function even after that outer function has returned. Lets go back to our above code and assign foo to the variable me.

```
var me = foo("Frankie");

me("Earth"); // This will return "Frankie says sup Earth"

```
The Javascript engine knows that in order for the bar function to run at a future time that it will need both the greeting variable and the name parameter within the foo function.  So it allows bar to inspect its environment and closes over the variables that it needs to remember for future use.  The references to these variables are closed in a special data structure that can only be accessed by the Javascript runtime itself.

As one begins to understand closures you may make the mistake in assuming that closures store copies of its outer function's variables. When in fact, they only maintain references to the outer functions variables. The cool factor behind this feature becomes more apparent when the value of the outer function's variable changes before the closure is called. Lets look at some more code to hone in this point.

```
function feelings() {
    var mood = "sad";
    // In this function we are returning an object with some inner functions
    // The inner functions have references to the outer function variables

    return {
        getMood: function() {
            // This function will return the current variable mood even after setMood
            //  function changes it
            return mood;
         },
         setMood: function(feeling) {
             // This function will change the outer variable anytime it is invoked
             mood = feeling;
         }
    }
}

var myMood = feelings(); // the feelings outer function has returned

myMood.getMood() ; // "Sad"
myMood.setMood("Happy"); //Changes the outer function's variable
myMood.getMood(); // "Happy"

```

Closures are one of Javascript's most powerful features. Being able to store references to outer functions that have already returned has many use cases. They can be a convenient way of reducing the the number of parameters passed to a function.  Another practical use case comes from the fact that because only the closure and only the closure has access to the variables within its outer environment you can use closures to achieve private variables and encapsulation in Javascript.  Mostly, it "just works" and functions transparently remember any variables needed for future execution in a convenient way.
