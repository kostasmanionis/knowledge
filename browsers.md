# Web browsers

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



