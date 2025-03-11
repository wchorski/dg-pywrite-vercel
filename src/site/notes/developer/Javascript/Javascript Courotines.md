---
{"dg-publish":true,"tags":["js","javascript","automation","async","yield"],"permalink":"/developer/javascript/javascript-courotines/","dgPassFrontmatter":true}
---

```js
function* test() {
    console.log('Hello!');
    var x = yield;
    console.log('First I got: ' + x);
    var y = yield;
    console.log('Then I got: ' + y);
}
```

To initiate an instance of a coroutine you just call the function, which returns a coroutine object:
```js
var tester = test();
```

So far none of the function will have executed, so we must execute the function until it hits a `yield`, by calling:
```
tester.next(); // prints 'Hello!'
```

Now `tester` is paused at the first `yield`, and we can resume the coroutine by sending a value to the `yield` where it is paused:
```js
tester.next('a cat'); // prints 'First I got: a cat'
```

Now `tester` is paused at the second `yield`, and we can send another value:
```js
tester.next('a dog'); // prints 'Then I got: a dog'
```

That's about all there is to coroutines—they are functions which can suspend themselves using the `yield` keyword, and you can communicate with them by sending values, which will be returned as the value of `yield` and resume execution.

## The Convenient `coroutine` Wrapper

Every time you use a coroutine you always do three things:

1. Call the function and store the resulting coroutine object.
2. Call `next()` on the coroutine object, to execute until the first `yield`.
3. After that all you can do is call `next(...)` to run the coroutine and send it values.

We can put all of this functionality into a wrapper function:
```js
function coroutine(f) {
    var o = f(); // instantiate the coroutine
    o.next(); // execute until the first yield
    return function(x) {
        o.next(x);
    }
}
```

This sets up the coroutine and returns a function which we can call directly instead of calling `next` on the coroutine object.

Using this wrapper, our earlier example becomes:
```js
var test = coroutine(function*() {
    console.log('Hello!');
    var x = yield;
    console.log('First I got: ' + x);
    var y = yield;
    console.log('Then I got: ' + y);
});
// prints 'Hello!'

test('a dog'); // prints 'First I got: a dog'
test('a cat'); // prints 'Then I got: a cat'
```

## A Coroutine Loop

Our first example coroutine yields twice and then ends. Let's make a never-ending coroutine instead.

Here's a coroutine that never ends, and each time you resume it, it alternates between ticking and tocking:
```js
var clock = coroutine(function*() {
    while (true) {
        yield;
        console.log('Tick!');
        yield;
        console.log('Tock!');
    }
});

clock(); // prints 'Tick!'
clock(); // prints 'Tock!'
clock(); // prints 'Tick!'
```

_Note that in Javascript, calling `clock()` with no argument is the same as calling `clock(undefined)`, which in this case is fine since this coroutine doesn't even look at the values being sent to it._

Now let's make the clock tick/tock once every second. It's very easy:
```js
setInterval(clock, 1000);
```

This resumes the coroutine once every 1000ms, and it will tick and tock for eternity.


---
## Credit
- [Coroutine Event Loops in Javascript (x.st)](https://x.st/javascript-coroutines/)