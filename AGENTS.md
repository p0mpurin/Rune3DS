# Agent Commit Rules

These rules are mandatory for AI agents and maintainers before committing or publishing Nocturne changes.

## Release and Version Rules

- If a commit changes release behavior, user-facing features, or the CIA users should install, bump the version.
- For every release version bump, update all of these together in the same commit:
  - `include/update.hh`: `VERSION_MAJOR`, `VERSION_MINOR`, `VERSION_PATCH`, and `VERSION_DESC`
  - `nocturne-version`: plain text version only, for example `1.5.32`
  - `NOCTURNE_CHANGELOG.md`: add a section for the new version
  - `README.md`: update release-facing text if install/update behavior changed
  - `universal-updater/nocturne.unistore`: update version, revision, and last_updated
- `nocturne-version` must match the compiled app version exactly.
- GitHub releases must include `3hs.cia`.
- Nocturne updates are handled through Universal-Updater. The in-app self-updater is disabled.
- Do not point Nocturne at the official 3hs CIA. Official 3hs updates are a compatibility signal only.

## Build and Publish Rules

- Before publishing a release, build with:

```sh
perl build.pl --target release
```

- After building, record or verify the SHA256 of `3hs.cia`.
- Tag releases with the matching version, for example `v1.5.32`.
- Mark the newest GitHub release as Latest.
- Do not overwrite an older release asset for a new version. Create a new tag and release.
- The FBI QR in `README.md` should point to `assets/nocturne-latest-fbi-qr.png`, which encodes the latest-release `3hs.cia` URL.

## Commit Hygiene

- Keep version bumps, changelog entries, and asset changes atomic in one commit.
- Do not commit local/auth files such as `source/hsapi_auth.c`.
- Do not commit root generated build outputs (`3hs.cia`, `3hs.elf`, `3hs.3dsx`).
