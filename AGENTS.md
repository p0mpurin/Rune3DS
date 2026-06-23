# Agent Commit Rules

These rules are mandatory for AI agents and maintainers before committing or publishing Nocturne changes.

## Release and Version Rules

- If a commit changes release behavior, user-facing features, updater behavior, bundled assets, or the CIA users should install, decide whether it needs a version bump before committing.
- For every release version bump, update all of these together in the same commit:
  - `include/update.hh`: `VERSION_MAJOR`, `VERSION_MINOR`, `VERSION_PATCH`, and `VERSION_DESC`
  - `nocturne-version`: plain text version only, for example `1.5.13`
  - `NOCTURNE_CHANGELOG.md`: add a section for the new version
  - `README.md`: update release-facing text if install/update behavior changed
- `nocturne-version` must match the compiled app version exactly. Do not release a CIA when these disagree.
- Future GitHub releases must include both assets:
  - `3hs.cia`
  - `nocturne-version`
- The launch updater downloads from GitHub's stable latest-release URLs:
  - `https://github.com/p0mpurin/just-an-hshop-fork/releases/latest/download/nocturne-version`
  - `https://github.com/p0mpurin/just-an-hshop-fork/releases/latest/download/3hs.cia`
- Do not point Nocturne auto-update at the official 3hs CIA. Official 3hs updates are only a compatibility/source signal because installing stock 3hs would overwrite this fork.

## Build and Publish Rules

- Before publishing a release, build with:

```sh
perl build.pl --target release
```

- After building, record or verify the SHA256 of `3hs.cia`.
- Tag releases with the matching version, for example `v1.5.13`.
- Mark the newest GitHub release as Latest, otherwise installed apps will keep seeing the previous release.
- Do not overwrite an older release asset for a new version. Create a new tag and release.
- The FBI QR in `README.md` should point to `assets/nocturne-latest-fbi-qr.png`, which encodes the latest-release `3hs.cia` URL. Regenerate it only if the repository or asset URL changes.

## Commit Hygiene

- Keep version bumps, changelog entries, and updater asset changes atomic in one commit when they belong to the same release.
- Do not commit local/auth files such as `source/hsapi_auth.c`.
- Do not commit generated build outputs (`3hs.cia`, `3hs.elf`, `3hs.3dsx`) unless the repository policy changes; publish CIAs as GitHub release assets instead.
