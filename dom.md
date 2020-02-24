The `querySelectorAll` method differs from methods like `getElementsByTagName` and `getElementsByClassName` in that these methods return an `HTMLCollection` which is a live collection, whereas `querySelectorAll` returns a NodeList which is static.

So if you would do `getElementsByTagName('p')` and one `<p>` would be removed from the document, it would be removed from the returned `HTMLCollection` as well.
But if you would do `querySelectorAll('p')` and one `<p>` would be removed from the document, it would still be present in the returned NodeList.
Another important difference is that an `HTMLCollection` can only contain `HTMLElements` and a `NodeList` can contain any type of `Node`.

```js
document.body.insertAdjacentHTML(
  "beforeend",
  '<a href="/home" class="active">Home</a>'
);
```

The `insertAdjacentHTML` method allows you to insert an arbitrary valid HTML string into the DOM at four positions, indicated by the first parameter:
'beforebegin': before the element
'afterbegin': inside the element before its first child
'beforeend': inside the element after its last child
'afterend': after the element

```html
<!-- beforebegin -->
<p>
  <!-- afterbegin -->
  foo
  <!-- beforeend -->
</p>
<!-- afterend -->
```

The standard DOM API also provides some handy methods to inspect the DOM. For example, matches determines if an element will match a certain selector:

```html
<p class="foo">Hello world</p>
```

```js
const p = document.querySelector("p");
p.matches("p"); // true
p.matches(".foo"); // true
p.matches(".bar"); // false, does not have class "bar"
```

You can also check if an element is a child of another element with the contains method:

```html
<div class="container">
  <h1 class="title">Foo</h1>
</div>
<h2 class="subtitle">Bar</h2>
```

```js
const container = document.querySelector(".container");
const h1 = document.querySelector("h1");
const h2 = document.querySelector("h2");
container.contains(h1); // true
container.contains(h2); // false
```

You can get even more detailed information on elements with the `compareDocumentPosition` method.

You can observe changes to any DOM node through the `MutationObserver` interface.