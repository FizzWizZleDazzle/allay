# Allay

A Minecraft launcher built with [Lumen](https://github.com/lumen-fx/lumen).

Allay aims to be easy to pick up and deep when you need it: install a version
and play in two clicks, or manage loaders, mods, and instances in detail.
Modpacks are described by what they contain, not by one pinned game version,
so a pack resolves against the version you play, and mods migrate forward
when the game updates. Allay changes nothing in the game unless you ask it
to.

Early in development. What works today: browsing every version Mojang
publishes, installing one, making instances of it, signing in to a Microsoft
account, and playing. Mod loaders and modpacks do not exist yet.

## Run from source

Install the Lumen toolchain from [lumenfx.dev](https://lumenfx.dev), then
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
select, and every object the version's asset index names, then unpacks the
native libraries. Nothing is verified against a checksum: Mojang publishes
SHA-1 digests and the download module checks SHA-256 only
([lumen-fx/lumen#298](https://github.com/lumen-fx/lumen/issues/298)).

The manifest is cached so the list is on screen before the network answers.
With no connection the page shows the cached list and when it was fetched;
with no cache either, it offers a retry.

## Instances

An instance is a name, a game version, and a heap size, with its own game
directory for saves and options. Play starts the JVM and streams the game's
output into the instance panel.

The game runs with the launcher's own working directory, because a Lumen
child process cannot be given one of its own
([lumen-fx/lumen#299](https://github.com/lumen-fx/lumen/issues/299)). Saves
and options follow the instance because they are addressed by `--gameDir`;
the game's log file and crash reports do not. Allay also cannot stop a game
it started ([lumen-fx/lumen#300](https://github.com/lumen-fx/lumen/issues/300)).

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
beside the app.
