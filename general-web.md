Use native form validation!

[Chrome about e-commerce sites, some great stuff there.](https://www.youtube.com/watch?v=F2GRAYyTF9Y) - might be useful for 1stdibs.com.

### When to www

The only technical reason is that if you use`http://example.com`, requests to subdomains \(eg`static.example.com`\) will be sent the cookies sent on the main domain. This is seen as a disadvantage because you might want to use a subdomain for serving static content, and the cookies add more overhead.

> This only happens when you set the cookie for .example.com \(notice the dot prefix\) domain name. If you set a cookie for example.com domain, www.example.com will not get/send that cookie.

However that’s not entirely true. It only makes a difference if you have enough cookies that they don’t fit in one packet \(~1kb\). And even then, it only really matters if you’re Google or Facebook.

Also with HTTP/2, using multiple subdomains \(aka “domain sharding”\) is actually detrimental - it’s faster to have everything on 1-2 domains max.

[Why use www](http://www.yes-www.org/why-use-www/)

### Infrastructure

[React repository infrastructure](https://reactjs.org/blog/2017/12/15/improving-the-repository-infrastructure.html)

[About why it's worth using proper html and css](https://alistapart.com/article/cult-of-the-complex)