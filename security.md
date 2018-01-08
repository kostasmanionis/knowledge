Hash flooding attacks

Hashtable lookups are O\(1\) in the best case, but O\(n²\) in the worst case \(i.e. only hash collisions\). Hash flooding attacks trigger the exponential complexity by sending precomputed data, where all keys hash to the same value. If this data somehow gets stored in a hashtable on the server-side, the attack takes effect. A handful of HTTP requests, each sending just a few KB of data, is enough to hang the server CPU for minutes. This attack is possible when the attacker knows the hashing function \(duh, open source\) and the hashing seed used on the server \(whoops!\). Seems that even random seed hash flooding is possible.

Find out about ssl pinning.

[A brief explanation about generating password hashes.](http://www.blinkingcaret.com/2017/11/15/things-wanted-know-storing-passwords-afraid-ask/)

[A brief explanation of https](http://www.blinkingcaret.com/2017/01/18/brief-ish-explanation-of-how-https-works/) - unread

[Putting the helmet on – Securing your Express app](https://www.twilio.com/blog/2017/11/securing-your-express-app.html)

[Why blindly add dependencies is dangerous](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5)

[Content Security Policy](https://developers.google.com/web/fundamentals/security/csp/)