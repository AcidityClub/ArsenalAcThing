# Arsenal - Rolve Anticheat
xonae don't dmca this please

just trying to teach people how the anticheat works, i am not promoting the bypass of RAC, nor violation of Roblox's
ToS for client modifications

## Clientside Checks
### VirtualUser Detection
If the "VirtualUser" service is found, you will be auto-banned via the "Client" LocalScript firing HitPart with
specific arguments (the argument in question is just a string saying "t").

The kick will display with the message set to "T2", attempting to throw off exploiters.

This detection catches certain scripts (Hydroxide) upon execution.

## Serverside Checks
### Hit Registration Verification (Raycast, Integrity Checks)
Arsenal added a system some time ago to detect shooting through walls (Wallbang) and blatant kill-all exploit scripts.

Whenever the "HitPart" RemoteEvent (the name actually contains special characters to throw off cheaters) is fired, it
contains a BitBuffer string with general data and (more importantly) position and look-vector values.

The server checks these by performing its own raycast, and seeing if the results match. There are also a few
extra safeguards in place to prevent easy exploitation of the HitPart RemoteEvent.

⦁	Type 1: Detects extremely wrong/suspicious values or behavior in regard to hit registration. For example,
a sub-check that is this type checks to see if the position of the player on the server-side and the position
of the client-reported position have deviated significantly.
⦁	Type 2: Detects less wrong-looking or suspicious-looking values and behavior. False positives are more
likely to happen to sub-checks of this type. High latency is more of an issue with type-2, but still
affects type-1 if it is EXTREMELY high.

Also, according to a few script developers that I've spoken to a little while back, if you fail these type-1/2
checks too often or too much (too many times probably), a delayed ban will be issued for you (it probably
bans at the end of the round, but I personally have not confirmed this).

The HitPart RemoteEvent (along with quite a few other sensitive remotes) use a method to see if you have
fired the remote with a series of two nil arguments at the end. Some RemoteSpy scripts cannot pick these nil
arguments up, causing script developers to fire the RemoteEvent WITHOUT the arguments, also resulting
in a slightly delayed ban.

In the BitBuffer (for HitPart) there is a bit value that tells the server if it's a critical, headshot,
backstab, stomp(?), and such. Certain integrity checks have been put into place to detect completely impossible
values. It will instantly ban you if you try to perform a headshot and backstab (tested).

(!?) If a (bypassing) Kill-All cheat is used, it will probably not work unless the used weapon is projectile-based,
in which it will COMPLETELY bypass server-sided checks.

Also, there are other checks that are hit-registration related, but are just simple integrity checks to shooting
dead players, or shooting people on spectator team and/or while you are dead.

### Fake RemoteEvents
In the game, there are various RemoteEvents that ban when they are fired. An example is the "Drop" RemoteEvent,
which allowed you to drop your weapon in the earlier stages of Arsenal. The feature was eventually removed, buta script was developed to spam the remote with requests to drop admin/developer exclusive weapons,
primarily the "Very Long Bat", which takes up a huge amount of space and lags the game if dropped
constantly.

After anti-cheats found out, they completely disabled functionality of the RemoteEvent, and it now serves no other
use, except for banning those using patched scripts.

### Weapon Spawner Detection
Before detections for weapon spawners were created and/or were implemented, people could easily exploit the
RemoteEvent responsible for giving a certain item to themselves (which I'm assuming is exclusive to Randomizer)
or by using the Dex Explorer method.

Essentially, you could freely use admin/developer exclusive weapons. The first time they added detections, it
was to make attacking with the weapon instant ban. The weapon was still equipped server-wise, meaning you could
take advantage of the "Burn" remote, and deal damage that way without issues.

Eventually, the equipping of any item with "AdminWeapon" in the data folder was made to instant ban.

### Invalid Team Detection
Self explainatory, firing the RemoteEvent with an invalid team or trying to join Purple/Orange team
without permissions will instant ban.
