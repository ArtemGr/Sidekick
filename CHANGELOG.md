## Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to [Semantic Versioning](http://semver.org/).

### [1.7.1] - 2019-09-16
`!` Communication between shards, handlers and database replicas moved from custom UDP to HTTP as a workaround for a `may` building [issue](https://github.com/ArtemGr/Sidekick/issues/195#issuecomment-511734402).  
`+` The [sum flag](https://github.com/ArtemGr/Sidekick/issues/181#issuecomment-531603458) tells `repeat` to sum the results.

### [1.7.0] - 2019-04-06
`+` The spaces are no longer squashed before the parsing, allowing us to implement space-using dice notations.

### [1.6.4] - 2019-03-11
`+` The "t" option can be used to count a face [twice](https://github.com/ArtemGr/Sidekick/issues/151).

### [1.6.3] - 2019-02-21
`+` The `:scrissors:` emoji (✂) tells Sidekick to delete it's message.

### [1.6.2] - 2018-11-13
`+` Multiple failure conditions: `/r 1d10>6f1f2f3`.

### [1.6.1] - 2018-10-17
`+` Mention the inquirer in the `/r deal` replies.

### [1.6.0] - 2018-07-08
`+` Patron registration implemented. Memorized roll limits lifted for the servers blessed by patrons.

### [1.5.9] - 2018-04-29
`!` Memorized rolls and statistics moved from a single PostgreSQL server to the replicated High Availability embedded SQLite pair.

### [1.5.8] - 2018-02-18
`+` Replicated (master-master, asynchronous, eventually consistent) SQLite implemented, providing a High Availability database layer.  
`+` The bot can now deal cards (#95): `/r deal 3`, `/r shuffle`.

### [1.5.7] - 2018-01-06
`!` High Availability shard failover reimplemented: handlers are used to synchronize usage statistics between the shards,
    allowing failover shards to connect whenever primary shards stop getting events.

### [1.5.6] - 2018-01-01
`+` Rerolls, 3d6r1.

### [1.5.5] - 2017-12-13
`+` OVA rolls.  
`+` Shards separated from handlers. UDP messaging between the shards and the handlers.

### [1.5.4] - 2017-09-24
`+` It is now possible to create a channel-wide memorized roll by using a capital letters name.

### [1.5.3] - 2017-07-09
`+` Added a command to list the memorized rolls (`/r $`).

### [1.5.2] - 2017-05-30
`+` Memorized rolls can now be removed.

### [1.5.1] - 2017-05-02
`+` AOL syntax (sponsored by Justin Dafonte).

### [1.5.0] - 2017-04-18
`+` Database integration with timeouts.  
`+` Ability to save rolls for later. /r $wp = 5. /r $wp.

### [1.4.2] - 2017-03-05
`+` High Availability failover implemented ([announcement](https://www.reddit.com/r/discordapp/comments/5xjqia/the_bots_on_high_and_available/)).  
`+` Bot instances are now more closely monotored by the watchdog.

### [1.4.1] - 2017-02-21
`+` We are now switched to a version of the driver (discord-rs) patched to use OpenSSL 0.9.

### [1.4.0] - 2017-02-11
`+` Basic LFG implemented. You can now look for like-minded players across the board.

### [1.3.0] - 2017-01-15
`+` Use a sharded connection to Discord.  
`+` Recurrent explosion ([#7](https://github.com/ArtemGr/Sidekick/issues/7)).  
`+` Fudge/Fate rolls ([#13](https://github.com/ArtemGr/Sidekick/issues/13)).  
`+` Patreon link.

### [1.2.0] - 2016-09-15
`+` WoD rolls ([#4](https://github.com/ArtemGr/Sidekick/issues/4) and [#5](https://github.com/ArtemGr/Sidekick/issues/5),
[announcement](https://www.reddit.com/r/discordapp/comments/53hdz1/wod_support_landed_in_sidekick/)).

### [1.1.0] - 2016-08-30
`+` Keeps and drops ([#1](https://github.com/ArtemGr/Sidekick/issues/1)).

### [1.0.0] - 2016-08-03
`+` Basic dice (3d6) and math.
