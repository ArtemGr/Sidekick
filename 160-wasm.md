
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
