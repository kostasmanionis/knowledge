[Use css grid ](https://www.youtube.com/watch?v=7kVeCqQCxlk)

[Prefers reduced motion](https://css-tricks.com/introduction-reduced-motion-media-query/)

How FireFox stylo works

[CSS Architecture for Modern JavaScript Applications](https://www.madebymike.com.au/writing/css-architecture-for-modern-web-applications/?utm_source=ponyfoo+weekly&utm_medium=email&utm_campaign=189)

## Layouts

[Masonry layouts](https://css-tricks.com/piecing-together-approaches-for-a-css-masonry-layout/)

[Single direction margins](https://csswizardry.com/2012/06/single-direction-margin-declarations/)

[Top 10 most deadly CSS mistakes made by new and experienced developers](https://www.painlesscss.com/top-10-css-mistakes.html)

#### Mistake #8: Using px units

🚫 Mistake

```css
p {
  font-size: 16px;
  line-height: 20px;
  margin-bottom: 8px;
}
```

✅ Correct

```css
body {
  font-size: 16px;
}
p {
  font-size: 1rem;
  line-height: 1.25;
  margin-bottom: 0.5em;
}
```

#### Mistake #6: Breaking the document flow

The tricky part about CSS is you can write different CSS code to achieve the same visual result. For example, to horizontally center an element on a page, you could use any of the following techniques, and they’ll all look the same to the user:

- Surround the element with &nbsp; until it looks centered
- Explicitly set the width and left margin of the element to a fixed value that makes it look centered.
- Use text-align: center
- Use margin: 0 auto
- Use a combination of floated siblings and a clearfix.
- Use a combination of position, left, and transform.
- Which technique is best? Depending on your goals, it may be perfectly correct to use any of the above techniques.

Generally speaking, there are some rules of thumb we try to follow to select the best layout method.

First, we eliminate any technique that uses absolute measures instead of relative measures. This means #1 &nbsp; and #2 explicitly setting the width are out.

Second, we prefer techniques that do not modify the document flow over techniques that break the document flow. This is because elements follow the document flow by default, so if you break this default assumption, you’ll make it harder for your co-workers (or yourself, 6 months from now) to understand what’s going on. This means #5 float and #6 position are out.

More generally, we prefer techniques that require as little change to other things as possible. Keeping your footprint small helps you avoid sprawling, complicated code that is hard to maintain.

Technique #3, text-align: center works only on inline elements, so we’ll choose #3 whenever our HTML is already an inline element, and we avoid the extra need to add a display: inline CSS declaration.

Technique #4, margin: 0 auto works only on block elements, so we’ll choose #4 whenever our HTML is already a block element, and we avoid the extra need to add a display: block CSS declaration.

Breaking the document flow too frequently (e.g. overzealously using float) increases your footprint and makes your layout harder to understand. When there are multiple options to write CSS code to achieve the same visual result, we prefer using techniques that have a smaller footprint.

#### Mistake #5: Not separating design from layout

🚫 Mistake

```css
.article {
  display: inline-block;
  width: 50%;
  margin-bottom: 1em;
  font-family: sans-serif;
  border-radius: 1rem;
  box-shadow: 12px 12px 2px 1px rgba(0, 0, 0, 0.2);
}
.sidebar {
  width: 25%;
  margin-left: 5px;
}
```

```html
<div class="article"></div>
<div class="article sidebar"></div>
```

✅ Correct

```css
/_ Layout _/ .article,
.sidebar {
  display: inline-block;
}
.article {
  width: 50%;
  margin-bottom: 1em;
}
.sidebar {
  width: 25%;
  margin-left: 5px;
}

/_ Lipstick _/ .card {
  font-family: sans-serif;
  border-radius: 1rem;
  box-shadow: 12px 12px 2px 1px rgba(0, 0, 0, 0.2);
}
```

```html
<div class="article card"></div>
<div class="sidebar card"></div>
```
