

### Streaming responses

Browsers have a buffer limit before rendering chunked responses, in most cases it's [~1kb](https://stackoverflow.com/questions/16909227/using-transfer-encoding-chunked-how-much-data-must-be-sent-before-browsers-s/16909228#16909228).

> In considering performance, you want to make sure that you're not producing way too chunky data. The more "chunking" you do, the more overhead that exists in both producing the chunks and parsing the chunks. Furthermore, it also results in more executions of buffering functions if the receiver can't make immediate use of the chunks. Chunking isn't always the right answer, it adds extra complexity on the recipient. So if you're sending small units of things that won't gain much from streaming, don't bother with it!

Chunked responses can be compressed.

> I believe that the content gets compressed first, and then cut up in chunks. That way, the chunks are received, then decompressed to acquire the real content. If it were the other way around, you'll get the compressed stream, and then decompressing would give us chunks. Which doesn't make sense.

Buffering

> At the NGINX level, buffering is dependent upon the type of the upstream connection. There are 3 common connection types for HTTP: "proxy", "uwsgi", "fastcgi". If you want your NGINX server to respect streaming, you can either switch off buffering for your connection type, or match the buffer size with the upstream chunk size. Switching off buffering can be done using a buffering directive \(`proxy_buffering, uwsgi_buffering, fastcgi_buffering)`, or you can use a special header`X-Accel-Buffering: no`which tells NGINX to not buffer the response. The special header is more flexible, as this allows NGINX to buffer responses that don't need streaming. It also works for all 3 connection types.

> There's an advantage in keeping buffers available or having a larger buffer size than the chunk size. It comes from dealing with slow clients. NGINX as a reverse proxy is very fast and can read the response from your upstream application server very quickly. NGINX itself can deal with any slow browsers that has a slower read rate than your upstream's write rate. Because NGINX is very light weight \(asynchronous IO\), the cost of holding a connection in NGINX is far smaller than holding open a process \(that is waiting for the client to finish reading\) in your application server. This is of course relative, as your application server might also be very light weight, and rely on either green threads or asynchronous IO. This problem does reveal an interesting property of streaming systems. Any stream will only be as quick as the slowest link \(reader or writer\) in the chain.

[Source](https://gist.github.com/CMCDragonkai/6bfade6431e9ffb7fe88).

### Backpressure

[Backpressure in NodeJS](http://engineering.voxer.com/2013/09/16/backpressure-in-nodejs/)

