# Nocturne Vercel Updater

This is a tiny static Vercel project kept for legacy updater testing.

Current Nocturne releases use Universal-Updater instead of the in-app updater.

It serves:

- `/version.txt`
- `/3hs.cia`

Before deploying a release:

1. Copy the built root `3hs.cia` to `vercel-updater/public/3hs.cia`.
2. Update `vercel-updater/public/version.txt` to match `include/update.hh`.
3. Commit both files and push to the Vercel-linked branch.

The mirrored `public/3hs.cia` is intentionally committed so Git-backed Vercel
deployments can serve the updater download without any manual dashboard upload.
