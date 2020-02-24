# Web browsers

### General

[Browser rendering path](https://fabianstiehle.com/render-path) - an introduction on how the browser rendering path works.

[Prevent default kills click modifiers](https://remysharp.com/2019/04/04/how-i-failed-the-a)

Stuff like ⌘+click
### Firefox

#### **Quantum CSS engine**

[New CSS engine called Quantum CSS](https://hacks.mozilla.org/2017/08/inside-a-super-fast-css-engine-quantum-css-aka-stylo/)

##### Parrarel processing

The Servo project \(which Quantum CSS comes from\) is an experimental browser that’s trying to parallelize all of the different parts of rendering a web page. Quantum CSS makes use of this recent feature of computers by splitting up style computation for the different DOM nodes across the different cores.

This might seem like an easy thing to do… just split up the branches of the tree and do them on different cores. It’s actually much harder than that for a few reasons. One reason is that DOM trees are often uneven. That means that one core will have a lot more work to do than others.

To balance the work more evenly, Quantum CSS uses a technique called work stealing. When a DOM node is being processed, the code takes its direct children and splits them up into 1 or more “work units”. These work units get put into a queue.

##### Speed up restyles with the Rule Tree

For each DOM node, the CSS engine needs to go through all of the rules to do selector matching. For most nodes, this matching likely won’t change very often. For example, if the user hovers over a parent, the rules that match it may change. We still need to recompute style for its descendants to handle property inheritance, but the rules that match those descendants probably won’t change

The CSS engine will go through the process of figuring out the selectors that match, and then sorting them by specificity. From this, it creates a linked list of rules. On restyle, the engine does a quick check to see whether the change to the parent could potentially change the rules that match children. If not, then for any descendants, the engine can just follow the pointer on the descendant node to get to that rule. From there, it can follow the tree back up to the root to get the full list of matching rules, from most specific to least specific. This means it can skip selector matching and sorting completely.

So this helps reduce the work needed during restyle. But it’s still a lot of work during initial styling. If you have 10,000 nodes, you still need to do selector matching 10,000 times. But there’s another way to speed that up.

##### Style sharing cache

Think about a page with thousands of nodes. Many of those nodes will match the same rules. For example, think of a long Wikipedia page… the paragraphs in the main content area should all end up matching the exact same rules, and have the exact same computed styles.

If there’s no optimization, then the CSS engine has to match selectors and compute styles for each paragraph individually. But if there was a way to prove that the styles will be the same from paragraph to paragraph, then the engine could just do that work once and point each paragraph node to the same computed style.

That’s what the style sharing cache—inspired by Safari and Chrome—does. After it’s done processing a node, it puts the computed style into the cache. Then, before it starts computing styles on the next node, it runs a few checks to see whether it can use something from the cache.

Those checks are:

* Do the 2 nodes have the same ids, classes, etc? If so, then they would match the same rules.
* For anything that isn’t selector based—inline styles, for example—do the nodes have the same values? If so, then the rules from above either won’t be overridden, or will be overridden in the same way.
* Do both parents point to the same computed style object? If so, then the inherited values will also be the same.

Those checks have been in earlier style sharing caches since the beginning. But there are a lot of other little cases where styles might not match. For example, if a CSS rule uses the`:first-child`selector, then two paragraphs might not match, even though the checks above suggest that they should.

In WebKit and Blink, the style sharing cache would give up in these cases and not use the cache. As more sites use these modern selectors, the optimization was becoming less and less useful, so the Blink team recently removed it. But it turns out there is a way for the style sharing cache to keep up with these changes.

In Quantum CSS, we gather up all of those weird selectors and check whether they apply to the DOM node. Then we store the answers as ones and zeros. If the two elements have the same ones and zeros, we know they definitely match.

# APIs

```js
'hello world'.bold().italics().link('http://localhost') // <a href="http://localhost"><i><b>hello world</b></i></a>
```
