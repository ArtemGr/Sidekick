## Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to [Semantic Versioning](http://semver.org/).

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
