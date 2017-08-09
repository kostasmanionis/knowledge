### Browser

[Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

### JavaScript

#### Factory functions

Don't expose a `new` -able constructor, use factory functions instead.

```js
export default init(options) {
    return new Constructor(options);
}
```

Helps you do clever optimisations inside the factory function.

#### Webpack like configuration

#### Helps with flexibility, though might add complexity.

```
minChunks: number|Infinity|function(module, count) -> boolean
```



