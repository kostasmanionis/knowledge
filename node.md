[Express error handling](https://nemethgergely.com/error-handling-express-async-await/)

[V8 code caching for node](https://github.com/zertosh/v8-compile-cache)

[Speeding up Node starup performance with snapshots](https://docs.google.com/document/d/1YEIBdH7ocJfm6PWISKw03szNAgnstA2B3e8PZr_-Gp4/edit#heading=h.677lbnx3xmo2)

#### Improved diagnostics trace events (Super time saver)

Debugging the code in 10.0.0 is super duper easy, now we have trace events that create a manual tracing to a category and when enabled pass the diagnostic code to a file which can be read by google chrome dev-tools. No more cli to be used for creating traces for code, now we have javascript API that helps enabling/disabling trace events dynamically.

[API](https://nodejs.org/api/tracing.html)