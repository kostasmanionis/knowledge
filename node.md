[Express error handling](https://nemethgergely.com/error-handling-express-async-await/)

[V8 code caching for node](https://github.com/zertosh/v8-compile-cache)

[Speeding up Node starup performance with snapshots](https://docs.google.com/document/d/1YEIBdH7ocJfm6PWISKw03szNAgnstA2B3e8PZr_-Gp4/edit#heading=h.677lbnx3xmo2)

#### Scaling node.js to 100k concurrent connections!

1. Nagle’s algorithm is disabled

If you’re familiar at all with real-time network programming, you’ll recognize this algorithm as a common socket tweak. This makes each response leave the server much quicker.

The tweak is available through the node.js API “socket.setNoDelay“, which is set on each long-poll COMET connection’s socket.

2. V8’s idle garbage collection is disabled via “–nouse-idle-notification”

This was critical, as the server pre-allocates over 2 million JS Objects for the network topology. If you don’t disable idle garbage collection, you’ll see a full second of delay every few seconds, which would be an intolerable bottleneck to scalability and responsiveness. The delay appears to be caused by the garbage collector traversing this list of objects, even though none of them are actually candidates for garbage collection.

`–nouse-idle-notification`

Turns of the idle garbage collection which makes the GC constantly run and is devastating for a realtime server environment. If not turned off the system will get a long hickup for almost a second once every few seconds.

`–expose-gc`

Use the expose-gc command to enable manual control of the GC from your code. I recommend to call GC once every 30 seconds.

`–max-old-space-size=8192`

Increases the limit for each V8 node process to use max 8Gb of heap memory instead of the 1,4Gb default on 64-bit machines(512Mb on a 32-bit machine).

`–max-new-space-size=2048`

Specified in kb and setting this flag optimizes the V8 for a stable allround environment with short pauses and ok high peak performance.

If this flag is not used the pauses will be a little bit longer but the machine will handle peaks a little bit better. What you need in this case depends on the project you are working on. My pick is to have an allround stable server instead of just handling peaks so I stick with this flag.

#### Improved diagnostics trace events (Super time saver)

Debugging the code in 10.0.0 is super duper easy, now we have trace events that create a manual tracing to a category and when enabled pass the diagnostic code to a file which can be read by google chrome dev-tools. No more cli to be used for creating traces for code, now we have javascript API that helps enabling/disabling trace events dynamically.

[API](https://nodejs.org/api/tracing.html)

[V8 PERF](https://github.com/thlorenz/v8-perf) - Repo about V8 performance

[Why the New V8 is so Damn Fast](https://nodesource.com/blog/why-the-new-v8-is-so-damn-fast) - Has some great tools about debugging node deoptimizations

#### Starting a new node project

```sh
npx license mit > LICENSE
npx gitignore node
npx covgen YOUR_EMAIL_ADDRESS
npm init -y
```

```sh
git init
npx license $(npm get init.license) -o "$(npm get init.author.name)" > LICENSE
npx gitignore node
npx covgen "$(npm get init.author.email)"
npm init -y
git add -A
git commit -m "Initial commit"
```

#### ES Modules

Add “type”: “module” to the package.json for your project, and Node.js will treat all .js files in your project as ES modules.

Node.js will treat the following as ES modules when passed to node as the initial input, or when referenced by import:

- Files ending in .mjs.
- Files ending in .js, or extensionless files, when the nearest parent package.json file contains a top-level field “type” with a value of “module”.
- Strings passed in as an argument to —-eval or piped to node via STDIN, with the flag —-input-type=module.

If some of your project’s files use CommonJS and you can’t convert your entire project all at once, you can either rename those files to use the .cjs extension or put them in a subfolder containing apackage.json with { “type”: “commonjs” }, under which all .js files are treated as CommonJS.

##### Exports field

A new package.json field “exports” can also define the main entry point, along with subpaths. It also provides encapsulation, where only the paths explicitly defined in “exports” are available for importing. “exports” applies to both CommonJS and ES module packages, whether used via require or import.
This allows statements like import ‘pkg/feature’ to map to a path like ‘./node_modules/pkg/esm/feature.js'. It also causes an error to be thrown for statements like import ‘pkg/esm/feature.js’ unless that path is defined in “exports”.
