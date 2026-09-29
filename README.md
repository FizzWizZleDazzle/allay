# Allay

A Minecraft launcher built with [Lumen](https://github.com/lumen-fx/lumen).

Allay aims to be easy to pick up and deep when you need it: make a profile and
play in two clicks, or pick its loader and mods in detail. A profile is a list
of mods, not a game version: you choose the version when you press Play, and
every mod is fetched in its build for that version, so the same profile plays
on an old release and on the newest one. Allay changes nothing in the game
unless you ask it to.

Early in development. What works today: browsing every version Mojang
publishes, profiles with Fabric or Quilt mods from Modrinth, signing in to a
Microsoft account, and playing.

## Run from source

Install the Lumen toolchain, 0.0.8 or newer, from
[lumenfx.dev](https://lumenfx.dev), then
unpack the runtime modules for your platform from the same release into
`~/.lumen`; Allay uses the filesystem, download, archive, and process modules
and will not do anything useful without them.

```sh
lumenc run .
```

Playing needs a `java` on your `PATH`. The home page says which one it found.

## Versions

The versions page browses every Minecraft version Mojang publishes. It shows
the latest release and the latest snapshot at the top, then the full list with
a type filter and a search box.

Installing one downloads the client jar, the libraries this system's rules
select, and every object the version's asset index names, each checked against
the SHA-1 Mojang publishes. A version older than 1.19 also has its native
libraries unpacked; newer ones load them from their jars. Play installs the
version it needs on its own, so a visit here is optional; Pick for Play sets
the version the Play dialog starts from.

A version counts as installed only while every file its launch reads is on
disk. One whose install stopped part way, or whose files have gone missing
since, shows Finish install, which fetches only what is missing; Play does the
same before it starts the game.

The manifest is cached so the list is on screen before the network answers.
With no connection the page shows the cached list and when it was fetched;
with no cache either, it offers a retry.

## Profiles

A profile is a name, a mod loader, a heap size, and a list of mods. It is not
tied to a game version. Press Play, pick a version, and Allay:

1. finds each mod's newest build for that version and loader on
   [Modrinth](https://modrinth.com), along with every mod those builds list
   as required. When a mod names the exact build of another it needs, that
   build is used;
2. shows you what it found before the game starts. A mod with no build for
   the version is left out, and you are shown which ones. A mod you mark
   Required stops the launch instead, and so do two mods that say they do not
   work together;
3. installs the game version if it is not installed yet, fetches the loader,
   and downloads the mods, each checked against the SHA-1 Modrinth publishes;
4. starts the game.

Every version a profile is played on gets its own game directory inside the
profile, holding its worlds, options, configs, and mods. A world made on one
version is never opened by another, so playing an older version cannot
downgrade it.

The loader is Vanilla, Fabric, or Quilt. A new profile is Vanilla; adding a
mod to it moves it to Fabric, since mods need a loader. Quilt runs Fabric
mods, so a Quilt profile takes a mod's Quilt build when there is one and its
Fabric build otherwise. Forge and NeoForge are not supported.

Allay removes only the mod files it put in a mods folder itself. A jar you
drop in there by hand stays, and loads alongside the profile's mods.

The game runs in its game directory, so its log and crash reports sit beside
its worlds, and its output streams into the profile panel. Stop the game ends
it. A game keeps running when Allay closes.

Resolving needs Modrinth: a modded profile does not start while Modrinth
cannot be reached. The loader falls back to the build already on disk for
that version when its own servers are down.

Instances from earlier Allay builds become profiles the first time it starts,
each keeping its worlds in the directory for the version it was made for.

## Accounts

Signing in uses Microsoft's device-code flow: Allay shows a code, you approve
it in a browser, and Microsoft hands back a token. Your password is never
entered into Allay.

Allay signs in as the Minecraft for Nintendo Switch application, because
Mojang no longer registers new launcher applications and a third-party
launcher has no client id of its own to offer.

Tokens are stored unencrypted in Allay's data directory. Lumen has no keyring
binding yet.

Without an account Allay launches in offline mode, which works in singleplayer
and on servers that do not check.

## Where things are kept

Everything Allay downloads and everything you change lives in one per-user
directory, reported at the bottom of the settings page. Nothing is written
beside the app. The game directory setting moves the profiles, with their
worlds and mods, somewhere else.
