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

The file `list.json` consists of a list "players", where each element always contain an `ip` field, and may contain a `playername` field. When a player attempted to join using authentication, their UUID is also stored

# Other information

Some people have tried to connect without setting a player name (`185.181.8.203` for instance), this caused an exception on the server
```
[20:15:36] [Server thread/INFO]: /185.181.8.203:55030 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[20:52:26] [Server thread/INFO]: /185.181.8.203:40572 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[22:12:49] [Server thread/INFO]: /185.181.8.203:45214 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[23:00:23] [Server thread/INFO]: /185.181.8.203:49206 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[19:46:23] [Server thread/INFO]: /185.181.8.203:36686 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[15:40:17] [Server thread/INFO]: /185.181.8.203:58566 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
[18:07:37] [Server thread/INFO]: /185.181.8.203:58754 lost connection: Internal Exception: io.netty.handler.codec.DecoderException: java.lang.IndexOutOfBoundsException: readerIndex(1) + length(1) exceeds writerIndex(1): PooledUnsafeDirectByteBuf(ridx: 1, widx: 1, cap: 1)
```
They tried many times with different ports each time!

# Some anti-bot solutions

## IP bans

This is the main purpose of this list, a.k.a. provide a list of IPs used for botting. The goal is not only to ban them from your minecraft server; but also from websites, web services, from sending mails, and more...
It is most likely that these IPs are from cheap rented VPSs that will likely be used for other activities, such as phishing, bruteforce ssh, mail spamming or automated scrapping.

## Enforce hostnames

The way scanners work means that they will not obtain the hostname to your server -- only it's ip.
Luckily, minecraft requires that players connecting to a server send the address they're using to connect.
In the case of scanners, they can only provide the IP that they obtained through port scanning, while players will give the hostname you sent them in order to connect.
Thus, you can prevent people from connecting -- or even resolving -- to your server by blocking players trying to do so by checking if they're trying to connect via IP

Below is a code fragment to put in your plugin (requires ProtocolLib)
```java
Game.getProtocolManager().addPacketListener(new PacketAdapter(
	(Plugin)this, ListenerPriority.HIGHEST, PacketType.Handshake.Client.SET_PROTOCOL)
{
	@Override
	public void onPacketReceiving(PacketEvent event)
	{
		PacketContainer hs = event.getPacket();
		server.getConsoleSender().sendMessage(hs.getStrings().read(0));
		String addr = hs.getStrings().read(0);

		if (!addr.equals("hostname.com")) // Custom condition
			event.setCancelled(true);
	}
})
```
Server will be listed as offline and players won't get a response after their handshake.
