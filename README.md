# Shinena Texts

The downloadable textbook packs for the **Shinena** Android app.

The app does not bundle the texts inside the APK — on first launch it downloads
them from here, unzips them into app storage and reads the chapters from disk.
The web build (`https://verba-master.vercel.app`) keeps the same texts bundled
and never touches this repository.

## What is in here

```
manifest.json      # version, corpus list, per-book metadata, checksums
packs/mishna.zip   # 63 books   (משנה)
packs/tanakh.zip   # 39 books   (תנ"ך)
packs/talmud.zip   # 37 books   (תלמוד בבלי)
packs/rambam.zip   # 11 books   (רמב"ם)
packs/shua.zip     # 4 books    (שולחן ערוך)
```

Every ZIP holds one JSON file per book — exactly the same `OfflinePack` shape
the app uses (`id`, `title`, `corpus`, `totalChapters`, `chapters[]`), so the
mobile app and the web app read identical data.

## Download URLs

```text
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/mishna.zip
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/tanakh.zip
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/talmud.zip
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/rambam.zip
https://github.com/sniffis-commits/shinena-texts/releases/download/v1/shua.zip
```

`manifest.json` lives next to them:

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
