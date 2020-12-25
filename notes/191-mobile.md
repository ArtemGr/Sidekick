
☐ Document the HTTP API and share it with Fraks

☑ Receive text input from mobile app  
☑ “/r bind app” on any private channel → generate a temporary (lm) bind key, return the public part  
→ ☐ If the user is not a patron, suggest getting a patron key  
☑ Check if the hash of the mobile message matches the stored hash of the public part  
☑ If it is then share the bind secret (or hash / public key) with the app  
☑ Send the secret with the polling requests  
☑ Check the secret while handling `Poll` in `fn mobile`  
☑ Refactor the schedule function to use the channel/author ID obtained  

☐ Upload screenshot points into Sidekick  
☐ Fuel certain schedule activities with points  
→ ☐ Reaction that marks an activity as point-based (at certain scale)?  
→ ☐ A different kind of reaction that also allows to go into minus? That way we can track the time spent together in a pairing activities that needs to later be reinbursed?  
☐ Should be able to see and edit the (recent) history of point transactions, in order for Sidekick to be useable as a general purpose ledger  

☐ Mention how Discord was down on 2019-12-07, https://status.cloud.google.com/incident/compute/19012  
  The app allows accessing the bot even when the Discord is down.
