## Tools

[Google H2 auto push](https://github.com/google/node-h2-auto-push) - This project is for automating server push and getting rid of the need for manual configurations from service developers. It is meant as a helper library for building middlewares for various Node.js web servers, such as Express, fastify, etc.

#### Blackhole servers

A blackhole server can be used to route third party traffic through an endpoint that effectively makes requests disappear, recreating the effects of a complete outage. 72.66.115.13 provided by webpagetest

> Because these types of assets block rendering, the browser will not paint anything to the screen until they have been downloaded \(and executed/parsed\). If the service that provides the file is offline, then that’s a lot of time that the browser has to spend_trying_to access the file, and during that period the user is left potentially looking at a blank screen. After a certain period has elapsed, the browser will eventually timeout and display the page without the asset\(s\) in question. How long is that certain period of time?
>
> It’s**1 minute and 20 seconds.**
>
> If you have any render-blocking, critical, third party assets hosted on an external domain, you run the risk of showing users a blank page for 1.3 minutes.

#### Critical path css

[Usus](https://github.com/gajus/usus) - Renders page using Chrome Debugging Protocol \(CDP\). Extracts CSS used to render the page. Renders HTML with the blocking CSS made asynchronous. Inlines the critical CSS.

[inline-critical-css](https://github.com/yoshuawuyts/inline-critical-css) - Stream that inlines critical CSS in HTML. Looks at the used CSS on a page and only inlines the CSS that's used.

#### Animations

#### [FLIP](https://aerotwist.com/blog/flip-your-animations/)

`will-change` - [https://github.com/Microsoft/monaco-editor/issues/426](https://github.com/Microsoft/monaco-editor/issues/426)

[120fps](https://dassur.ma/things/120fps/)

#### Bundling

[bundle-buddy](https://github.com/samccone/bundle-buddy) - Bundle Buddy is a tool to help you find source code duplication across your javascript chunks/splits. This enables you to fine tune code splitting parameters to reduce bundle invalidation rates as well as improve repeat page load performance \o/.

[esbench](https://esbench.com/) -

#### Techniques

[Use `this.Reflektive` to feature detect 2015](https://itnext.io/a-bloatless-web-d4f811c7991b)

[Skeleton screens](https://medium.com/@owencm/reactive-web-design-the-secret-to-building-web-apps-that-feel-amazing-b5cbfe9b7c50) - skeleton screens ensure that whenever the user taps any button or link, the page reacts immediately by transitioning the user to that new page and then \_loading in content to that page \_as the content becomes available.

#### Perceived performance

[Designing for mobile performance](http://www.awwwards.org/brainfood-mobile-performance-vol3.pdf)

Most of the time it's more important to make the app feel fast, rather than making it fast for real.

Humans overestimate passive waits by 36%, per Richard Larson of MIT

If your app is slow, and you focus on improving objective speed, it'll have to be 20% faster than before for the difference to even be noticeable. Which means around 30% is needed to make it feel like an impactful improvement.

##### Managing user wait time

* Respond immediately.
* Let users know where they are in the waiting process; how much time is elapsed and how much is left to go.
* Especially with long waits, keeping user attention focused on other things.
* Render minimum viable layout, to get users out of a passive phase and into an interactive, usable page ASAP

Postulated by business consultant David Maister but confirmed in several studies. If people don't have a sense of how much has elapsed and how much is left, the wait feels longer.

##### Clicks

Replace click events with mousedown \(and touchstart\) events. Click events actually fire not when the mouse button goes down, but when it comes back up again. So there's this gap when the user has clicked down, but the click event has yet to fire. We can load things when the mouse button goes down, and only display them when the click event fires as the user releases their mouse button. Gives about a 100-150ms head start.

##### Progress bars

Chris Harrison, a grad student at Carnegie Mellon found that progress bars with bands that animate in the opposite direction of the progress make the bar seem to fill faster. Same principle of being on a train, and another train is coming by the opposite direction, you look out the window to see the other train whizzing by, and it seems like you're going twice as fast.

In a followup study he found that if those bands accellerate, the bar feels faster still. Can get that accelerating feel just by applying an ease-in to the animation. He found that they feel 12% faster.

* Any action taking less than 1 or so seconds doesn't need an indicator at all, thought is uninterrupted. By showing an indicator here, you're taking their focus away from the action they just took and forcing them to focus on the fact that they have to wait for the app to respond. So you are in effect making the app feel \*slower\*.
* 1-3 seconds displaying a progress bar is preferable. Don't need to include lots of written explanation as to what the app is doing, since this is within the window where most people still have patience.
* 3-10s? Include an explanation as to what the app is doing. These durations can seem unreasonable if you don't explain to the user what is happening.

**Spinners**

Spinners have a pretty narrow use case. Don't want to use them if the expected wait time is less than 1s or so, because user flow won't be disrupted. Don't want to use them if the expected wait time is ~2.5s or up, since users need certainty as to where they are in the process. So you're looking at using them for loads between 1 - 2ish seconds.

With long waits, up to and past 10 seconds. Sometimes you just have to distract users and keep them from focusing on how long the wait is. Games frequently give players tips to read during long loading screens. Tips require high levels of mental activity to read them, digest them, and integrate them into the player's existing play style. Thus holding attention. Uber's old screen when it was matching you with a driver was interactive. You could draw little light doodles in this creative play space. If you know the wait is going to be \*very\* long, give users something interactive to play with, because nothing holds attention like play.

## At the end of the day, what matters is how it feels.

If you have sufficient low hanging fruit to effect a 30% speedup in your app then by all means, do it. But many team don't have that luxury, and for them it makes a great deal of sense to focus on subjective performance measures before dedicating significant resources on physically making their product faster.

## Above the fold size

Due to how TCP estimates the capacity of a connection (i.e. TCP Slow Start), a new TCP connection cannot immediately use the full available bandwidth between the client and the server. Because of this, the server can send up to 10 TCP packets on a new connection (~14KB) in first roundtrip, and then it must wait for client to acknowledge this data before it can grow its congestion window and proceed to deliver more data.

Also, it is important to note that the 10 packet (IW10) limit is a recent update to the TCP standard: you should ensure that your server is upgraded to latest version to take advantage of this change. Otherwise, the limit will likely be 3-4 packets!

Due to this TCP behavior, it is important to optimize your content to minimize the number of roundtrips required to deliver the necessary data to perform the first render of the page. Ideally, the ATF content should fit under 98KB - this allows the browser to paint the page after just three roundtrips to have plenty time budget for server response latency and client rendering.

## Frameworks

### React performance

[Using should component update](https://medium.com/@alexandereardon/performance-optimisations-for-react-applications-b453c597b191)

[Benchmarking server side applications](https://davidea.st/articles/measuring-server-side-rendering-performance-is-tricky)

### Ember

[Glimmer binary](https://engineering.linkedin.com/blog/2017/12/the-glimmer-binary-experience)

### Images

[Images guide](https://images.guide/) - long ass book about image optimization.

[Image decoding](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/MbXp16hQclY/bQjegyrbAgAJ)

[Using SVG as image placeholders](https://medium.freecodecamp.org/using-svg-as-placeholders-more-image-loading-techniques-bed1b810ab2c)

```html
<!-- the decode for this image may be deferred -->
<img decoding=async src="...">

<!-- if possible the decode for this image should not be deferred -->
<img decoding=sync src="...">

<!-- the browser is free to do what it feels is best for the user -->
<img decoding=auto src="...">
```

[Chrome proposal for lazyloading images and iframes](https://css-tricks.com/a-native-lazy-load-for-the-web-platform/)

Sending data in a comment might be the fastest way to do it, need to benchmark parsers.

### GIF

[Safari TL ships an image tag that let's you load mp4 as images. A superb alternative to gifs.
](https://calendar.perfplanet.com/2017/animated-gif-without-the-gif/)

### Metrics

[How Wiki Media foundation tracks performance regressions](https://calendar.perfplanet.com/2017/automating-web-performance-regression-alerts/)

[Client-side Performance Monitoring with InfluxDB](https://tech.wayfair.com/2018/07/client-side-performance-monitoring-with-influxdb/)

[A step by step guide to monitoring the competition with the Chrome UX Report](https://dev.to/rick_viscomi/a-step-by-step-guide-to-monitoring-the-competition-with-the-chrome-ux-report-4k1o)

## Browsers

[Fastdom - Batch dom read/writes](https://github.com/wilsonpage/fastdom)

[Using the cache api to switch between inline and cached files](https://www.filamentgroup.com/lab/inlining-cache.html)

[Don't block the page unload event by using Beacon API](https://www.smashingmagazine.com/2018/07/logging-activity-web-beacon-api/)

[An analysis of Chromium's paint timing metrics](https://speedcurve.com/blog/an-analysis-of-chromiums-paint-timing-metrics/)

### Hacks

Messing with the priority queue![](/assets/Screen Shot 2017-11-26 at 19.29.34.png)

### Chrome

[JavaScript startup performance](https://medium.com/reloading/javascript-start-up-performance-69200f43b201)

[User experience report](https://developers.google.com/web/tools/chrome-user-experience-report/) - big query data set for getting performance metrics for certain websites.

[The cost of JS](https://medium.com/dev-channel/the-cost-of-javascript-84009f51e99e) - points out some common performance problems that browsers have when loading up js files.

## Case studies

[Treebo](https://medium.com/dev-channel/treebo-a-react-and-preact-progressive-web-app-performance-case-study-5e4f450d5299) - great study of a fast web app.

[Can you afford a performance budget](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/)

[Pinterest](https://medium.com/@addyosmani/a-pinterest-progressive-web-app-performance-case-study-3bd6ed2e6154)

[Tinder](https://medium.com/@addyosmani/a-tinder-progressive-web-app-performance-case-study-78919d98ece0)

[Start Performance Budgeting](https://medium.com/@addyosmani/start-performance-budgeting-dabde04cf6a3)

## CSS

[Reducing the size of css class names](https://medium.freecodecamp.org/reducing-css-bundle-size-70-by-cutting-the-class-names-and-using-scope-isolation-625440de600b)

[Using the Cache API to cache inlines styles](https://www.filamentgroup.com/lab/inlining-cache.html?utm_source=ponyfoo+weekly&utm_medium=email&utm_campaign=142)

[Loading fonts using JS](https://www.filamentgroup.com/lab/js-web-fonts.html)

### Split css files by media queries

```html
<link rel="stylesheet" href="all.css" media="all" />
<link rel="stylesheet" href="small.css" media="(min-width: 20em)" />
<link rel="stylesheet" href="medium.css" media="(min-width: 64em)" />
<link rel="stylesheet" href="large.css" media="(min-width: 90em)" />
<link rel="stylesheet" href="extra-large.css" media="(min-width: 120em)" />
<link rel="stylesheet" href="print.css" media="print" />
```

[More about loading css files](https://csswizardry.com/2018/11/css-and-network-performance/)

`A browser will not execute a <script> if there is any currently-in flight CSS.`

```html
<link rel="stylesheet" href="slow-loading-stylesheet.css" />
<script>
  console.log("I will not run until slow-loading-stylesheet.css is downloaded.");
</script>
```

```html
<!-- This JavaScript executes as soon as it has arrived. -->
<script src="i-need-to-block-dom-but-DONT-need-to-query-cssom.js"></script>

<link rel="stylesheet" href="app.css" />

<!-- This JavaScript executes as soon as the CSSOM is built. -->
<script src="i-need-to-block-dom-but-DO-need-to-query-cssom.js"></script>
```

## Networking

[Use the network information api to figure out what kind of an connection the user has](https://googlechrome.github.io/samples/network-information/)

[HTTP2 - what no one is telling you](https://www.youtube.com/watch?v=CkFEoZwWbGQ)

[Using HAR for performance benchmarking](https://blog.201-created.com/you-can-only-change-what-you-can-measure-6be8826503a7)
