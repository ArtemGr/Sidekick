https://github.com/ArtemGr/Sidekick/issues/232

☑ Build on a Google Compute Engine instance, `gift-one`  
☒ `shard_to_handler` should talk to “orion2046”  
☑ Start the handler and shards  
☑ Build the binary locally (under the WSL), rsync to the server (cargo.md / musl)  
→ ☑ Remove hyper and websocket from discord-rs in order to proceed with musl  
→ ☑ Figure which dependencies result in linking errors  
→ ☑ openssl MUSL  
☐ Fix `rate limit checking error`  
☐ Implement token grab  
→ ☑ Login locally  
→ ☒ Login on the server  
→ ☑ Semi-automatic upload and restart  
→ ☐ Workaround reCAPTCHA  
→ → ☑ Setup xpra  
→ → ☑ Install NodeJS  
→ → ☐ Run the grab  
→ → ☐ Autostart the grab  
→ → ☐ Disable the local grab  
→ ☑ Periodic local `check`  
→ ☐ Send a warning automatically when close to the end of the reconnect counter  
→ ☑ Use the new driver in `check`, to see if the token is valid  
☐ Use a browser to exchange reactions with the bot, automatically testing liveliness and response time?  
☒ Experiment with proactive websocket restarts: What if reconnecting websocket allows Cloudflare to do rolling updates? Proactive restart might result in a shorter downtime then  
→ ☒ See if proactive `Regenerate` resets the counters ⇒ It does not!  
→ ☑ Make sure the shards are disabled on secondary hosts ⇒ Removed completely the code and binaries from other hosts, but the counter still goes, with zero messages in our logs, suggesting that some hidden reconnects might be happening in the driver  
☐ rsync the token from local, load it from the file  
→ ☐ Changing the token should result in reinitializing the handler  
☐ Cron to auto-start the handler and the shards  
☐ An optional script to upload the binary to orion2048 and fulda121 also  
☐ Enable the orion2048 “check”: disable the shards there, see if the handler databases sync  
☐ Reimplement a pause around `run_disco_shard` websocket reconnects  
☐ Reimplement shard activity tracking to work without the SQLite  
☐ Reimplement the :scissors: to work without the SQLite  
☐ Reduce the amount of RAM used by the shards  
☐ Rotate the logs, /dev/shm/gift/gift_disco_shard.log, ~/Sidekick/log  
☐ Support RESUME  
☐ See if we are a [very large bot](https://discord.com/developers/docs/topics/gateway#sharding-for-very-large-bots)  
☐ Look into hitting `Too many open files`

# google compute

https://console.cloud.google.com/compute/instances?project=api-project-193635906495  
gift-one is defined in “c:\Windows\System32\drivers\etc\hosts”  
ssh Artemciy@gift-one

Impressions from using the Google Compute Engine so far. The user interface is intuitive. Connection is all right, even better than before. The server feels nice. The Rust builds is slower, partly due to building in release mode, but also because we have less cores and the considerably websocket load  
The build under the load approaches ten minutes! ⇒ Looks like much less CPU is used with the shard SQLite code disabled!  
Looks like we'll need a more powerful instance. Or maybe we should build under the WSL and simply upload the binary

2020-12-13: Looks like there are websocket restarts, but likely happening not as often as when we were working from Germany, for the bot was working for half a day without any

2020-12-17: Uptime is 5 days but there is no /dev/shm and the gift processes are gone. Was it some kind of soft reboot? Looks like with Google Compute it's better to log to disk, and maybe move the other state files there as well…  
2020-12-21: Another instance of /dev/shm files vanishing. Maybe there's a cron active that cleans that folder one and then? Interestingly, the bot and the shards are still active and functional

2020-12-19: Hitting the OOM killer likely  
Might try to disable the killer with https://stackoverflow.com/questions/35791416/how-to-disable-the-oom-killer-in-linux, or maybe we should not

2020-12-23: Hitting `Too many open files`, should look into it

    lfg:1245] 13:02:35] talk_back_step error: lfg:1217] attempting to read `/dev/shm/gift` resulted in an error: Too many open files (os error 24)

# ws resume

“Right now - that limit is 1,000 sessions (meaning successfully calling IDENTIFY via the websocket -RESUMEs do not count)” - https://github.com/discord/discord-api-docs/issues/215

https://discord.com/developers/docs/topics/gateway#resuming

    Discord has a process for "resuming" (or reconnecting) a connection that allows the client to replay any lost events from the last sequence number they received in the exact same way they would receive them normally

    Your client should store the session_id from the Ready, and the sequence number of the last event it received. When your client detects that it has been disconnected, it should completely close the connection and open a new one (following the same strategy as Connecting). Once the new connection has been opened, the client should send a Gateway Resume:

    { "op": 6,
      "d": {
        "token": "my_token",
        "session_id": "session_id_i_stored",
        "seq": 1337 } }

    It's also possible that your client cannot reconnect in time to resume, in which case the client will receive a Opcode 9 Invalid Session and is expected to wait a random amount of time—between 1 and 5 seconds—then send a fresh Opcode 2 Identify.

    When you close the connection to the gateway with the close code 1000 or 1001, your session will be invalidated and your bot will appear offline. If you simply close the TCP connection, or use a different close code, the bot session will remain active and timeout after a few minutes. This can be useful for a reconnect, which will resume the previous session.

# discord at google

“Nop, we don't use AWS for anything. GCE for our API and a bunch of third-party providers for voice servers” - https://www.reddit.com/r/discordapp/comments/4e0ftx/whos_hosting_the_servers/d1wf9pd  
cf. https://cloud.google.com/compute  
→ https://cloud.google.com/blog/products/data-analytics/redshift-to-bigquery-migration-for-gaming-app “due to both technical and business reasons, we migrated completely to BigQuery”; “cost of network ingress/egress between Google Cloud and AWS”  
→ https://www.cloudflare.com/case-studies/discord “In turn, Discord, a Google Cloud customer, both provides customers with a snappier application through the high speed interconnections between Cloudflare and Google Cloud” (“We have Cloudflare sit in front of our websockets servers to absorb Layer 7 attacks and various layer 3 & 4 reflection attacks. We have 2.4 million concurrent users connected through Cloudflare to us, and Cloudflare quickly and securely serves our traffic even with spikes of websocket events up to 2 million/second”)

“Discord however chooses (for monetary and complexity reasons) to run out of a single one of these regions/zones for the entire service (us-east1-b)” - https://www.reddit.com/r/discordapp/comments/e7huer/discord_outage_7_december_2019/fa09tlf

“Backend wise we're primarily Python (stateless) and Elixir (stateful / realtime), but we have some Go and even C++ thrown in there when they fit the problem. Some of our older blog posts go into some better detail about how we architect things (https://blog.discordapp.com/tagged/engineering) if your interested! Happy to answer any other specific questions as I can :)” - https://www.reddit.com/r/Games/comments/95x7hw/discord_announces_the_discord_store/e3wgo6f

https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f  
https://blog.discord.com/how-discord-handles-two-and-half-million-concurrent-voice-users-using-webrtc-ce01c3187429  
https://blog.discord.com/how-discord-maintains-performance-while-adding-features-28ddaf044333  
https://blog.discord.com/how-discord-resizes-150-million-images-every-day-with-go-and-c-c9e98731c65d  
https://blog.discord.com/scaling-elixir-f9b8e1e7c29b  
https://blog.discord.com/how-discord-indexes-billions-of-messages-e3d5e9be866f  
https://blog.discord.com/how-discord-stores-billions-of-messages-7fa6ec7ee4c7  
https://blog.discord.com/how-discord-handles-push-request-bursts-of-over-a-million-per-minute-with-elixirs-genstage-8f899f0221b4

# cloudflare ws disconnects

“Logs from tcpdump show that Cloudflare sends a TCP reset after 1-5 minutes, despite both client and server being in sync on packets sent in each direction” - https://community.cloudflare.com/t/websockets-disconnected-in-aws-tokyo/44680  
→ https://news.ycombinator.com/item?id=11638081: “If you’re intending to use CF websockets, be prepared for random (and potentially massive) connection drops, and be sure you’re architected to handle these disconnects gracefully. Cloudflare rolling restarts have caused hundreds of thousands of websocket connections to be disconnected in a matter of minutes for us”; “when terminating a WebSocket connection due to releases Cloudflare now signals this action to both client and origin server by sending the 1001 status code”

“When Cloudflare releases new code to its global network, we may restart servers, which terminates WebSockets connections” - https://support.cloudflare.com/hc/en-us/articles/200169466-Using-Cloudflare-with-WebSockets#12345687
