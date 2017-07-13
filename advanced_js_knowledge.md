## MVC

**Model**: Manages data for an application. When a model changes(when it is updated), it will notify its observers(e.g views)

**View**: Visual representation

**Controller**: Controller listens the events of the view and manipulates model. Model updates view. It is in a single direction.

There are generally 2 kinds of MVC pattern. One is view listening user's interaction, one is controller listening

```
view -> controller -> model -
  ^                          |
  |                          |
   --------------------------
```

![mv* patterns](./images/patterns.png)


## MVP
The change of `mode` would be catched by the `presenter`, and `presenter` updates the `view`.
![mvp pattern](./images/mvp.jpg)

## MVVM

The only difference between MVP and MVVM is that `MVVM` is using two-wat data binding: the change of view will directly reflect on the viewModel and vice versa.

![mvp pattern](./images/mvvm.jpg)

UI领域大致分化为：界面， 数据，事件，业务

## Memory Leak

[Reference 1](https://medium.com/outsystems-experts/beyond-memory-leaks-in-javascript-d27fd48ae67e)

[Reference 2](https://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/)


In essence, memory leaks can be defined as memory that is not required by an application anymore that for some reason is not returned to the operating system or the pool of free memory. Memory leaks happen when your code needs to consume memory in your application, which should be released after a given task is completed… but isn’t.

JavaScript is a `garbage collected` language. The **main cause** for leaks in JS is *unwanted reference*

### Garbage Collection
Garbage collection is an automatic memory management process that operates in runtime. It uses a `mark-and-sweep` algorithm that tracks all the paths and finds all objects in the object graph tree that are referenced from their roots.

### Three types of JavaScript Leaks
1. Accidental global variables

```ts
function foo(arg) {
    bar = "this is a hidden global variable";
}
```

```ts
function foo() {
    this.variable = "potential accidental global";
}

// Foo called on its own, this points to the global object (window)
// rather than being undefined.
foo();
```

One common cause for increased memory consumption in connection with globals are caches). Caches store data that is repeatedly used. For this to be efficient, caches must have an upper bound for its size. Caches that grow unbounded can result in high memory consumption because their contents cannot be collected.

Solution: The simplest way to avoid this is to enforce `use strict` directive. Strict mode doesn’t allow you to use undeclared variables.

2.  Closures (timers (setTimeout or setInterval).)

Any object inside the timer will hold a reference in order to run that piece of code somewhere in the future without any problems.

For example:

```ts
var myObj = {
    callMeMaybe: function () {
        var myRef = this;
        var val = setTimeout(function () {
            console.log('Time is running out!');
            myRef.callMeMaybe();
        }, 1000);
    }
};

//if we then run myObj.callMeMaybe() and then set myObje=null, the timer will still fire.
```


3. Lingering DOM References to Removed Elements

A DOM element belongs to the DOM, but it also exists in the Object Graph Memory. For example:

```ts
//Even elementToDelete is removed from the DOM, it is still referenced within the listener (the allocated memory for the object is still used)
var trigger = document.getElementById("trigger");
var elem = document.getElementById("elementToDelete");

trigger.addEventListener("click", function() {
    elem.remove();
});
```

### Ways to Identify Memory Leaks with Chrome DevTools

1. Performance tab

(Click the basket case icon (it starts the garbage collection process manually.)) After stopping the recording, we can check if JS Heap return to the initial allocated memory value

2. Snapshot compare (Memory tab)

click `Take Heap Snapshot`  (take snapshots) and serch for detach and then check for the difference.