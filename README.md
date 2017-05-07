# Sidekick
Superior dice bot for Discord.

If you've found a bot that's more awesome at rolling dice, file a bug!

## How to install

Just follow this link:
https://discordapp.com/oauth2/authorize?&client_id=209040908673482753&scope=bot&permissions=0

The link will prompt you to authorize the bot on a server. Once the bot's authorized, you'll see it in the Member List. In a public channel just type any of the commands outlined below and the bot will answer with a dice roll.

## Using it

`/r 1d8 + 4d6` - Roll one octahedron and four hexahedrons.

`/r 1d20+5 # Grog attacks` - Roll dice with a comment.

`/r 2d6>=5` - Roll two hexahedrons and take only the ones that turned greater or equal to five (aka difficulty check). Prints the number of successes.

`/r 4d6=5` - So can this guy roll five?

`/r 3d10>=6f1` - oWoD roll: rolling *one* is a failure, rolling more failures than successes is a *botch*.

`/r 4dF` - [Fudge/Fate dice](http://rpg.stackexchange.com/questions/1765/what-game-circumstance-uses-fudge-dice).

`/r 3d6!` - Exploding dice.

`/r 1d10!>9` - Explode nine and ten.

`/r 1d20!1k1` - Roll twenty, reroll on one (because halflings are lucky).

`/r 3d10!>=8` - nWoD roll: tens explode, eights and up are treated like a success.

`/r 4d6k3` - Roll four hexahedrons and keep the highest three (D&D 5e ability roll).

`/r repeat (4d6k3, 6)` - Roll D&D 5e ability score six times (to generate a new character).

`/r 2d20kl1` - Roll twice and keep the lowest roll (D&D 5e disadvantage).

`//roll-dice3-sides999` - AOL syntax. Dice noir.

`/r (2+2)^2` - Do math.

`/r 4d6^2` - Do math with dice.

## You can save the rolls for later!

`/r $persuasion = 2d20k1+3` - Remember the roll.

`/r $persuasion` - Use the memorized roll again.

## We need your support

If you want to help me spend more time on the bot, help your fellow players get better features fast,
or simply want to thank me and share a cup of beer,
please brave a visit to [my Patreon page](https://www.patreon.com/user?u=4695668)!

## Charts

![servers](https://blooks.today/r/sidekick-servers.gif)

```
```
[![Analytics](https://ga-beacon.appspot.com/UA-83241762-1/README)](https://github.com/igrigorik/ga-beacon)
