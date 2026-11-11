# About this fork

A fork of [kikootwo/ReadMeABook](https://github.com/kikootwo/ReadMeABook) (v1.3.0) that adds two features. Everything else is upstream.
Both were developed with AI assistance (Claude) and tested on a real library (about 12,000 books, 970+ requests).

## What is different

| Feature | What it does | Doc |
|---|---|---|
| **AudioSilo backend** | [AudioSilo](https://audiosilo.app/server) as a third library backend next to Plex and Audiobookshelf: library sync, matching, covers, scan after import | [documentation/features/audiosilo-integration.md](documentation/features/audiosilo-integration.md) |
| **Import lists** | An admin adds public Goodreads "Listopia" lists once; RMAB requests the audiobooks that are missing, with filter profiles, a preview, a free-space guard and an hourly request limit. Also an import-time check that refuses a download far too small for the book | [documentation/backend/services/list-sync.md](documentation/backend/services/list-sync.md) |

## Branches

| Branch | Contents | Use it for |
|---|---|---|
| `feature/audiosilo-backend-v130` | v1.3.0 + AudioSilo only | a PR or review of AudioSilo alone |
| `feature/import-lists-v130` | v1.3.0 + Import lists only | a PR or review of Import lists alone |
| `stack/lists-plus-audiosilo-v130` | v1.3.0 + both | running both together (what this fork runs) |

The two features do not overlap: both apply to v1.3.0 without conflicts. Import lists work with every backend, because the owned-book check reads RMAB's own library table.

## Trying it

```bash
git checkout stack/lists-plus-audiosilo-v130
docker build -f dockerfile.unified -t readmeabook-local:audiosilo .
```

Run it like the normal image. In the setup wizard choose **AudioSilo**, enter the server URL and a personal API key
(AudioSilo: account menu → Account → Personal API keys), and pick the library. Existing Plex and Audiobookshelf installs are unaffected.

## Known limits (AudioSilo)

- One backend per install, as with Plex and Audiobookshelf. Switching an existing install means changing `system.backend_mode`
  (a database setting) and restarting: nothing is migrated automatically, and old library rows stay until removed by hand.
- AudioSilo has no delete endpoint, no per-user ratings (BookDate has no rating scope) and no lookup by ASIN.
- AudioSilo has no filesystem watcher; the setup wizard turns on "scan after import" so new books show up at once.
- The first library sync of a 12,000-book library takes about 25 minutes.

## Status

Proposed upstream. Links to the pull requests or discussion go here when they exist.
