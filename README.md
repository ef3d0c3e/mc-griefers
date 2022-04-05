# List of minecraft griefers' accounts & address

This repository is a list of minecraft griefers.

The IPs are not meant for doing anything illicit. They're meant for blacklisting, i.e. blocking e-mails received from these IPs, blocking traffic to your website, minecraft servers, etc...
As they are most often associated to cheap VPS rented for the purpose of doing malicious actions.

Sometimes, there are no UUIDs associated with a certain user, that is because they are using a cracked minecraft account.
However, since some scanners will always use these playername, you can be almost certain that it's related to griefers.

# How

I set up a server that waits for players to join it.
Since the server's ip is not public and this ip has never ever before be used to host minecraft servers, you can be almost certain that the players attempting to join are griefers using an ip scanner.

# Format

The file `list.json` consists of a list "players", where each element always contain a `playername` and `ip` field. When a player attempted to join using authentication, their UUID is also stored
