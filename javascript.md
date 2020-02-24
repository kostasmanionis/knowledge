[Tilde](https://www.joezimjs.com/javascript/great-mystery-of-the-tilde/)

[Javascript wormholes](https://www.nearform.com/blog/wormholes-in-javascript/?utm_source=ponyfoo+weekly&utm_medium=email&utm_campaign=131)

## Garbage collection

### General Introduction

#### V8

The garbage collector is not one single action. Memory is divided up into two areas: the young generation and the old generation.

V8 partitions its managed heap into generations where objects are initially allocated in the “nursery” of the young generation. Upon surviving a garbage collection, objects are copied into the intermediate generation, which is still part of the young generation. After surviving another garbage collection, these objects are moved into the old generation. V8 implements two garbage collectors: one that frequently collects the young generation, and one that collects the full heap including both the young and old generation. Old-to-young generation references are roots for the young generation garbage collection. These references are recorded to provide efficient root identification and reference updates when objects are moved.

![fill](./assets/js-gc-gen.png)

Variables in JavaScript are mostly short-lived, used for a split-second for a single purpose and then discarded again. The young generation is specially designed to make this as fast as possible so your code is not held up. All new variables are allocated memory here, and organised into half-megabyte pages.

The young generation is partitioned into two semi-spaces. When a semi-space becomes full, a "minor" garbage collection begins. Variables are determined to be "living" or "dead" in a process called a scavenge. "Dead" variables are "unreachable", they were declared inside a function that has run its course and can no-longer be traced to living data. These variables are discarded. The minor garbage collection can take as little as 1ms, and the more dead data is discarded the more efficient the minor GC is.

![fill](./assets/js-gc-gen-1.png)

"Living" variables that are still being used are moved to the second semi-space. Any living variables that are still being used after they've already been moved once are moved to the "old generation".

The old generation is designed for longer-lived data. Unlike the young generation, which is a small, agile part of memory, the old generation can expand to a very large size. When the size of the whole heap grows large enough, a major garbage collection begins.

The major GC undertakes a root-and-tip survey of the whole heap to find dead data. This process is called "marking", and its job is to find data that is not needed and mark it to be reclaimed. The more data that is stored in the older generation, the longer marking takes. This is significant because marking can pause your application, causing it to be unresponsive. Marking the heap can take up to 100ms, or six frames of animation, for very large applications. This latency is one of the major reasons why JavaScript struggles with high-performance applications such as videogames, and why webassembly is so exciting for the future.

V8, Chrome's implementation of JavaScript, breaks this process up into smaller 1ms chunks so that most applications keep running without any noticeable pauses. The V8 team has recently introduced concurrent marking, allowing around two-thirds of this process to happen in the background while your application keeps running.

The heap is then "swept", which makes memory usable again. Sweepers also run in the background while your application is running, so that your application can keep running.

Memory is compacted throughout garbage collection. The empty space left in pages of memory means data can be re-organised to make more efficient use of space, and reduces the amount of memory the browser uses.

The garbage collector then is a program running within the browser that frees up memory used by unreachable data. It assumes that all unreachable data is not needed and can be discarded. Problems with memory occur when we fill it up with data we don't need, or when data never becomes unreachable and memory is not freed. These problems are called memory bloat and memory leaks.

[Stolen from Katie Fenn](http://www.katiefenn.co.uk/memory-dont-forget-to-take-out-the-garbage/)

##### More info

[Orinoco: young generation garbage collection](https://v8project.blogspot.lt/2017/11/orinoco-parallel-scavenger.html)

[Jank Busters Part One](https://v8project.blogspot.lt/2015/10/jank-busters-part-one.html)

[Jank Busters Part Two](https://v8project.blogspot.lt/2016/04/jank-busters-part-two-orinoco.html)

[Fall cleaning: Optimizing V8 memory consumption](https://v8project.blogspot.lt/2016/10/fall-cleaning-optimizing-v8-memory.html)

[JavaScript Performance Pitfalls in V8](https://ponyfoo.com/articles/javascript-performance-pitfalls-v8?utm_source=ponyfoo+weekly&utm_medium=email&utm_campaign=157)

Try to avoid mixing field values of different types, i.e. don’t mix numbers, strings, objects, other primitives, unless that’s what you intended to do. Specifically don’t pre-initialize number fields to null or undefined, but choose sensible default numbers (use NaN if in doubt). And try to initialize double fields (aka fields that are supposed to hold number values outside the small integer range) with double values outside the small integer range up-front, i.e. if in doubt put a NaN there first, and then store the actual initial value.

```sh
$ node --print-bytecode add10.js
…
[generated bytecode for function: add10]
Parameter count 2
Frame size 8
   19 E> 0x279c523204b2 @    0 : a5                StackCheck
   26 S> 0x279c523204b3 @    1 : 0b                LdaZero
         0x279c523204b4 @    2 : 26 fb             Star r0
         0x279c523204b6 @    4 : 25 02             Ldar a0
   34 E> 0x279c523204b8 @    6 : 34 fb 00          Add r0, [0]
         0x279c523204bb @    9 : 26 fb             Star r0
         0x279c523204bd @   11 : 25 02             Ldar a0
   36 E> 0x279c523204bf @   13 : 34 fb 01          Add r0, [1]
         0x279c523204c2 @   16 : 26 fb             Star r0
         0x279c523204c4 @   18 : 25 02             Ldar a0
   38 E> 0x279c523204c6 @   20 : 34 fb 02          Add r0, [2]
         0x279c523204c9 @   23 : 26 fb             Star r0
         0x279c523204cb @   25 : 25 02             Ldar a0
   40 E> 0x279c523204cd @   27 : 34 fb 03          Add r0, [3]
         0x279c523204d0 @   30 : 26 fb             Star r0
         0x279c523204d2 @   32 : 25 02             Ldar a0
   42 E> 0x279c523204d4 @   34 : 34 fb 04          Add r0, [4]
         0x279c523204d7 @   37 : 26 fb             Star r0
         0x279c523204d9 @   39 : 25 02             Ldar a0
   44 E> 0x279c523204db @   41 : 34 fb 05          Add r0, [5]
         0x279c523204de @   44 : 26 fb             Star r0
         0x279c523204e0 @   46 : 25 02             Ldar a0
   46 E> 0x279c523204e2 @   48 : 34 fb 06          Add r0, [6]
         0x279c523204e5 @   51 : 26 fb             Star r0
         0x279c523204e7 @   53 : 25 02             Ldar a0
   48 E> 0x279c523204e9 @   55 : 34 fb 07          Add r0, [7]
         0x279c523204ec @   58 : 26 fb             Star r0
         0x279c523204ee @   60 : 25 02             Ldar a0
   50 E> 0x279c523204f0 @   62 : 34 fb 08          Add r0, [8]
         0x279c523204f3 @   65 : 26 fb             Star r0
         0x279c523204f5 @   67 : 25 02             Ldar a0
   52 E> 0x279c523204f7 @   69 : 34 fb 09          Add r0, [9]
   55 S> 0x279c523204fa @   72 : a9                Return
Constant pool (size = 0)
Handler Table (size = 0)
…
```

```sh
$ node --trace-opt add10-optimized.js
[marking 0x32fc83347751 <JSFunction add10 (sfi = 0x32fcde157b91)> for optimized recompilation, reason: small function, ICs with typeinfo: 10/10 (100%), generic ICs: 0/10 (0%)]
[compiling method 0x32fc83347751 <JSFunction add10 (sfi = 0x32fcde157b91)> using TurboFan]
[optimizing 0x32fc83347751 <JSFunction add10 (sfi = 0x32fcde157b91)> - took 0.823, 0.514, 0.016 ms]
[completed optimizing 0x32fc83347751 <JSFunction add10 (sfi = 0x32fcde157b91)>]
```

##### V8 natives

[Inspect V8 internal data, data structures, internal types](https://github.com/NathanaelA/v8-Natives)

[Natives syntax list](https://gist.github.com/totherik/3a4432f26eea1224ceeb)

#### Decorators

Great for binding class properties!

```javascript
class Form extends Component {
  value = ""

  @bound
  submit() {...} // as function declaration, on prototype

  @bound
  change() {...} // as function declaration, on prototype

  change = () => {...} // as function assignment, instance

  render() {
    <div>
      <input onChange={this.change} value={this.value} type="text" />
      <button onClick={this.submit}>Click</button>
    </div>
  }
}
```

Validations too!

```javascript
const stringValidator = target => {
  const { descriptor } = target;
  const value = descriptor.initializer();

  const proxy = new Proxy(
    {},
    {
      set(proxyTarget, key, proxyValue) {
        if (typeof proxyValue !== "string") {
          throw new TypeError("value must be a string");
        }

        return true;
      }
    }
  );

  const initializer = () => {
    proxy[name] = value;
    return proxy[name];
  };

  return {
    ...target,
    initializer
  };
};

class Book {
  @stringValidator
  bookOne = "The Trial"; // all fine

  @stringValidator
  bookTwo = 1984; // throws a TypeError
}
```

[Implementing equality in javascript is difficult](https://medium.com/@modernserf/the-tyranny-of-triple-equals-de46cc0c5723)

However, there’s a number of implementation difficulties and edge cases that complicate this:

- How do you compare objects with circular references?
- If objects have identical properties, but different prototypes, are they equal?
- How should get/set properties be handled?
- Should objects compare their non-enumerable properties?
- Keys in an object are ordered — i.e. Object.keys({ x: 1, y: 2 }) gives different results than Object.keys({ y: 2, x: 1 }) -- should that matter for structural equality?
- How should you handle private properties, or methods that use closures to simulate this?

[Generators and Iterators](https://jfet97.github.io/JavaScript-Iterators-and-Generators/)
