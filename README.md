# Shinena Texts

The downloadable textbook packs for the **Shinena** Android app.

The app does not bundle the texts inside the APK — on first launch it downloads
them from here, unzips them into app storage and reads the chapters from disk.
The web build (`https://verba-master.vercel.app`) keeps the same texts bundled
and never touches this repository.

## What is in here

```
manifest.json      # version, corpus list, per-book metadata, checksums
packs/mishna-1.zip # 63 books   (משנה)
packs/tanakh-1.zip # 20 books   (תנ"ך)
packs/tanakh-2.zip # 19 books
packs/talmud-1.zip … talmud-6.zip (37 books)
packs/rambam-1.zip # 11 books   (רמב"ם)
packs/shua-1.zip   # 2 books    (שולחן ערוך)
packs/shua-2.zip   # 2 books
```

Every ZIP holds one JSON file per book — exactly the same `OfflinePack` shape
the app uses (`id`, `title`, `corpus`, `totalChapters`, `chapters[]`), so the
mobile app and the web app read identical data.

A corpus is split into **chunks** so the app never has to unpack more than
8 MB of text at once — cheap phones stay happy. `manifest.json` lists which
books live in which chunk.

## Download URLs

```text
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/mishna-1.zip
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/tanakh-1.zip
…
```

`manifest.json` lives next to them — the app always reads it first and then
downloads exactly the chunks it still needs:

```text
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/manifest.json
```

## How these files are built

The source of truth is the frontend repository
(`sniffis-commits/shinena-frontend`, `src/data/offline/*.json`). The ZIPs are
produced by `tools/build-text-packs.mjs` in that repo:

```bash
node tools/build-text-packs.mjs --out text-packs
```

That writes `manifest.json` + one ZIP per corpus, which are then uploaded as
the assets of the `v1` release.

## Versioning

`manifest.json` carries a `version` number. When it changes, the app re-downloads
only the corpora whose checksum changed — a text update never forces a full
re-download.

## Text sources

The texts are the public-domain Sefaria corpus
(<https://www.sefaria.org>). They are redistributed here so the app can work
fully offline after the first download.
