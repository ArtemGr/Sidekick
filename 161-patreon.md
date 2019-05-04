cf. https://github.com/ArtemGr/Sidekick/issues/161
patreon next step, we need to send a message into the channel, we know the channel id

`+` find a growth point, a place where to send the message from
    => `fn benefit_csv` adds the patrons to the `disco_patron_keys`,
       we should probably iterate over it from a method wired into `fn handle_disco_message` for now.

`+` in `fn handle_disco_message` catch "/reminders" at ARTEMCIY_DICE_PRIVATE_CHANNEL

`+` call `fn aga_patreon_reminders`

`+` iterate over the `disco_patron_keys`

`+` send the message

`-` insert that message into the schedule table as well, but only after it has been sent to Discord (i think)

`?` add a *page* describing how to *join* the Sidekick server (invite link) and then *talk* with the bot

--- with Dark Eclipse -------

`+` use `ChannelInfo::kind` in `fn dice_shard_worker` to forward private communication with Sidekick to the handler