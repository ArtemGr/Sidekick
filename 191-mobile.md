☐ Register the application with the bot

    ephemeral-channel: robot (ephemeral secret) <-> ciphertext <-> application (ephemeral secret)
    robot (public key) <- a part of a new ephemeral secret <- ephemeral-channel <- application (private key)
    robot (private key) -> ephemeral-channel -> a part of a new ephemeral secret -> application (public key)

    For simplicity and reuse, all communication between the application and the bot is encrypted with an ephemeral secret (so it is never simply discarded but only replaced with a new one). Also it offers a measure of protection against a man-in-the-middle attack.

    TABLE (session_id, ephemeral_secret, discord_uid)

  → ☐ See if we can use WASM from Flutter/Dart, this will allow us to implement the encryption once (in Rust).
    ⇒ https://pub.dev/packages/puppeteer, https://pub.dev/packages/chrome_dev_tools - headless Chrome.
    ⇒ https://pub.dev/packages/js - official JS interopt, part of the SDK, don't know which engine they hitch.
      ^ https://api.dart.dev/stable/2.3.1/dart-js/dart-js-library.html - documents the above?
      ^ https://pub.dev/documentation/js/latest/js_util/js_util-library.html
        "dart:js is soft deprecated, and package:js should be used" ??
      Looks like we'll have to try it.
  → ☐ Handle a registration command.
  → ☐ Upon the registration generate a small *session ID* with a small *ephemeral secret* and share them with the user (as a single number maybe).
  → ☐ In the application provide a way to enter the *ephemeral secret* once.
  → ☐ Generate the private/public pair in the bot and share the public part with the application by using the *ephemeral secret* channel.
  → ☐ Generate the private/public pair in the application and share the public part with the bot by using the *ephemeral secret* channel.
  → ☐ Use the assymetric cryptography to generate a new *ephemeral secret* (and *session ID*).

☑ Receive text input from mobile app  
☐ “/r bind app” on any private channel → generate a temporary (lm) bind key, return the public part  
→ ☐ If the user is not a patron, suggest getting a patron key  
☐ Check if the hash of the mobile message matches the stored hash of the public part  
☐ If it is then share the bind secret (or hash / public key) with the app  
☐ Send the secret with the polling requests  

☐ Upload screenshot points into Sidekick  
☐ Fuel certain schedule activities with points  
→ ☐ Reaction that marks an activity as point-based (at certain scale)?  
→ ☐ A different kind of reaction that also allows to go into minus? That way we can track the time spent together in a pairing activities that needs to later be reinbursed?  
☐ Should be able to see and edit the (recent) history of point transactions, in order for Sidekick to be useable as a general purpose ledger  

☐ Mention how Discord was down on 2019-12-07, https://status.cloud.google.com/incident/compute/19012  
  The app allows accessing the bot even when the Discord is down.
