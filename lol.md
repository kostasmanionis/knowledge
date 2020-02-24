[https://www.stefanjudis.de/hidden-messages-in-javascript-property-names.html](https://www.stefanjudis.de/hidden-messages-in-javascript-property-names.html)

[Dirty coding tricks](https://www.gamasutra.com/view/news/310570/Developers_share_their_most_memorable_dirty_coding_tricks.php)

[Debugging an evil Go runtime bug
](https://marcan.st/2017/12/debugging-an-evil-go-runtime-bug/)

Software saved the aerospace industry. Every other way of adding cost to an airplane also adds weight.

#### Void

Second, it’s a nice way to call immediately invoked functions:

```js
void (function() {
  console.log("What");
})();
```

All without polluting the global namespace:

```js
void (function aRecursion(i) {
  if (i > 0) {
    console.log(i--);
    aRecursion(i);
  }
})(3);

console.log(typeof aRecursion); // undefined
```

Since void always returns undefined, and void always evaluates the expression next to it, you have a very terse way of returning from a function without returning a value, but still calling a callback for example:

```js
// returning something else than undefined would crash the app
function middleware(nextCallback) {
  if (conditionApplies()) {
    return void nextCallback();
  }
}
```

Which brings me to the most important use case of void: It’s a security gate for your app. When your function is always supposed to return undefined, you can make sure that this is always the case.

```js
button.onclick = () => void doSomething();
```

So void and undefined are pretty much the same. There’s one little difference though, and this difference is significant: void as a return type can be substituted with different types, to allow for advanced callback patterns:

```ts
function doSomething(callback: () => void) {
  let c = callback() // at this position, callback always returns undefined
  //c is also of type undefiend
}


// this function returns a number
function aNumberCallback(): number {
  return 2;
}

// works 👍 type safety is ensured in doSometing
doSomething(aNumberCallback)
```