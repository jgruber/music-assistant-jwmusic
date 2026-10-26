# JW.org Music for Music Assistant

A [Music Assistant](https://music-assistant.io) custom music provider that
browses the publicly-published music catalog at
[jw.org/library/music-songs](https://www.jw.org/en/library/music-songs/) and
plays each track from the official MP3 URL the site serves on its public
download pages. No account required, no scraping — the provider talks to the
same `pub-media` JSON endpoint the site's own download buttons use.

## What it does

- Mirrors every top-level publication from the music-songs page as a
  browsable album: *"Sing Out Joyfully" to Jehovah* (and its Meetings,
  Vocals, and Instrumental variants), *Become Jehovah's Friend — Sing With
  Us*, *International Music*, *Original Songs*, *Become Jehovah's Friend —
  Original Songs*, the *Good News According to Jesus* soundtracks,
  *"Commit Your Way to Jehovah"*, *Sing to Jehovah* (and its New Songs and
  Chorus variants). The list is in
  [`constants.py`](jwmusic/constants.py) — add a publication by adding
  one line.
- Full dynamic search across every album by title or category — the first
  search warms the cache in parallel, every call after that is in-memory
  until the 24-hour TTL expires.
- Direct MP3 playback from the official file URLs the API returns.

## Install

### A. As a Music Assistant custom provider (requires upstream support)

Once Music Assistant core ships the `custom_providers/` loader (proposed in
[`feat/custom-providers`](https://github.com/jgruber/server/tree/feat/custom-providers)),
drop the `jwmusic/` directory from this repo into:

```
<storage_path>/custom_providers/jwmusic/
```

No Python dependencies to install — the provider uses only MA's bundled
`aiohttp`. Restart Music Assistant and the provider shows up with no
further setup.

### B. As a Home Assistant add-on (current working path)

Use the
[Boombox + Mixtape + JW.org Music add-on repo](https://github.com/jgruber/music-assistant-boombox-mixtape-sources) —
it bakes all three providers into the Music Assistant image. This is the
recommended install today; it will remain supported after the custom-
provider loader lands.

## Config

The provider has no mandatory configuration. There are no API keys, no
tokens, no accounts. It fetches an English-language catalog; if you want
another language, open an issue — the pub-media endpoint supports every
language the site publishes in.

## Icon

The provider ships with Music Assistant's default `music-note-eighth` MDI
icon. If you prefer the jw.org icon shown on the site, download it from
the live site and drop it next to the provider's `manifest.json` as
`icon.png`:

```
<storage_path>/custom_providers/jwmusic/icon.png
```

The provider owner (you) is responsible for your use of that image under
jw.org's terms of use; this repo doesn't bundle it.

## Safety

A custom provider runs with Music Assistant's full privileges. This one
makes outbound HTTPS requests to `b.jw-cdn.org` and plays MP3s from
whatever URL that endpoint returns. Only install custom providers from
sources you trust.

## License

Apache License 2.0 — see `LICENSE`.
