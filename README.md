# Allay

A Minecraft launcher built with [Lumen](https://github.com/lumen-fx/lumen).

Allay aims to be easy to pick up and deep when you need it: install a version
and play in two clicks, or manage loaders, mods, and instances in detail.
Modpacks are described by what they contain, not by one pinned game version,
so a pack resolves against the version you play, and mods migrate forward
when the game updates. Allay changes nothing in the game unless you ask it
to.

Early in development; it does not launch the game yet. What runs today is the
window itself: a sidebar, a home screen, and a settings screen whose theme,
Java memory, and game directory are saved to `allay.json` beside the app.

## Run from source

Install the Lumen toolchain from [lumenfx.dev](https://lumenfx.dev), then:

```sh
lumenc run .
```
