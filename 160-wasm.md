
☑ Upload WASMs into channels  
☑ `fn dice_io`: Load the set of WASM plugins for the channel.  
☑ → implement `fn wasm_plugins`  
☒ could] Get some positive V8 vibe from https://youtu.be/njt-Qzw0mVY.  
☑ `fn dice_io`: Explore WASM interpretation (inline function).  
☑ `fn dice_io`: Pass a WASM handler to `fn dice`.  
☐ `fn dice`: Run over the `input`, executing the WASM handler and the nom_inline_dice_notation.  
☒ → Communicate with Node.js via stdin/out pipe. ⇒ Hard to do around blocking I/O and with proper failure modes.  
☒ → https://github.com/cloudflare/quiche  
☑ → → Commit wasm-server.js for future reference and mention it in quic.md  
☑ Consider reusing some of the [Cloudflare docs and tools](https://workers.cloudflare.com/docs/tutorials/build-a-rustwasm-function/) to program the WASM  
☑ → How about using the Cloudflare workers to serve the WASM in the first place? That is, instead of running the Node.js locally we'll simply host the WASMs as the Cloudflare workers! Meaning that the users will be able to develop Sidekick extensions with any HTTP server, Cloudflare workers is simply a solution we'll use as an example and for our own development experiments ⇒ They're working fine, though one needs to proxy a site via the Cloudflare to get the workers.  
☑ → Evaluate https://aws.amazon.com/blogs/opensource/rust-runtime-for-aws-lambda/  
    They have [VSCode extension](https://aws.amazon.com/visualstudiocode/)  
    They have S3 and DynamoDB and Simple Notification Service  
    ⇒ Amazon created an EC2 instance to host the lambda, which has to be kept up and running for the lambda function to work (and was pain in the ass to disable), which is kind of different from on-demand computing I expected of it. And it's weird that they seem to take money for both the instance and the lambda computing time, and 100ms minimum at that.  
☐ → Try Node.js on Google App Engine; and/or https://cloud.google.com/functions/docs/  
☐ → Try https://glitch.com/create  
☐ → Try http://heroku.com/ (https://github.com/emk/heroku-buildpack-rust)  
☒ Communicate with Node.js via HTTP  
☐ → Unit test connecting to Node.js  
☐ Move shard-handler communication to HTTP, https://github.com/ArtemGr/Sidekick/issues/195  
☑ → Hyper server, [fast](https://www.techempower.com/benchmarks/#section=data-r18&hw=ph&test=plaintext)  
☑ → Port `ShardStatusPt::dispatch`  
☑ Move SQLite synchronization to HTTP  
☐ Move mobile-handler communication to HTTP  
  We should eventually use something like the https://pub.dev/packages/firebase_messaging to get notifications, in order to save battery and traffic and RAM, but for now we can just reuse the bidirectional HTTP communication, leaving the push optimization for later  
☑ → [Argo tunnel](https://github.com/cloudflare/cloudflared)  
☐ → Long polling  
☐ A command to register HTTP plugins  
☐ Drop the `wasm_bytes` table  

# Sketching deduplication

plugin 1 [ v5 function [ library functions ] ]
plugin 2 [ function [ library functions ] ]
plugin 3 [ function 1-10 KiB [ library functions 1-10 MiB ] ]
plugin 100th
plugin 200th
plugin 400th
...

Deduplication, because the same wasm can be uploaded to different channels/servers
could] Usage statistics, to differentiate between the oft used and the unused WASMs
could] Compress the wasm files that are used less often in order to reduce the CPU load
could] Given the potentially large size, consider using a separate database for the WASMs.
  Downside is having to setup in the firewall a second replication port.

A layer of dictionary compression should be designed (with implementation adjourned for later)
in hopes of uncluttering the database from the repeated WASM parts (library functions)
and for reuse in similar scenarios.

Expected queries:
UPSERT INTO wasm (channel, server, user, name, len, hash)  -- Mind wasm_bytes removal!
UPSERT INTO wasm_bytes (len, hash, body)
SELECT len, hash FROM wasm WHERE channel = ?
SELECT name FROM wasm WHERE channel = ?
When a WASM is removed *or updated*, we should check if any links remain to the old WASM:
SELECT channel FROM wasm WHERE len = ? AND hash = ?  -- Index on `len`?

# Running WASM

## Node.js

Going to use Node in order not to build V8 (we're gettting it with Node instead).
This should be more compatible with how the kind of enviroment most Rust-JS interoperations expect.

Windows version installed from https://nodejs.org/en/download/current/, using the latest version in order to get the fresh V8 with it (see the table at https://nodejs.org/en/download/releases/). Allowed Node to install the extra C++ development tools.

Embedding Node in Rust is, in turn, [a problem](https://github.com/nodejs/node/issues/24028), since the shared library is not released by default. So we're going to run Node as an independent process instead.

The stdin / stdout pipe is optimized in Linux, AFAIK. Should work as well as shared memory RPC.

In order to reduce the Rust-Node-WASM overhead we're going to send the entire bot command to WASM, getting back a fully processed result (e.g. with dice notation expressions replaced by rolls).
This processed result should be able to mark the replaced portions as final, preventing further tempering (standard dice natation, variable substitution) on the Rust side.

Should keep in mind that WASM processing ought to play well with variable substitution.

P.S. There is "node.dll" in Upwork, we can probably just use it on Windows!
