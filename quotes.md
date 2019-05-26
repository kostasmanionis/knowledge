```
The problem with Convention
Systems that try to contain complexity over long periods of time by convention will inevitably tend toward entropy, because one significant characteristic of convention is that it is trivially simple to break one.

You do not even need to be malicious. A convention is not a line in the sand. You can have a very good case for breaking or stretching one, but once a convention is no longer fully observed, subsequent cases for breaking or stretching it are automatically stronger, because the convention is already weakened. The more this happens, the weaker it gets.

In our case, the conventions that were in place to encourage simplicity became hazy. For example, our Sass is meant to follow to BEM, but in instances where the design requirements would create too many selectors, it defaults back to the cascade.

Advertisement

At the same time, any edits are assumed to be sympathetic to performance, because that has long been another convention. But an OKR team dedicated to moving a revenue metric might reasonably justify privileging conversions over page speed or longer-term development, especially if the feature is short-lived or the trade-off is small.

The consequence of our reliance on convention to manage this is that our Sass has become too hard to work with, and CSS too big to send down the wire.
```

from [Revisiting the rendering tier](https://www.theguardian.com/info/2019/apr/04/revisiting-the-rendering-tier)