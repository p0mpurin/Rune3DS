# Nocturne

A modern Nintendo 3DS fork of 3hs with an OLED-focused interface, custom wallpapers, faster downloads, and quality-of-life improvements.

> Nocturne is an unofficial community fork. It is not maintained or supported by the hShop team.

## Highlights

- OLED-black and light-pink visual redesign with refined colour palette
- Smooth eased selection, popup, and progress animations
- Crisp pixel-aligned typography with readable text sizing
- Custom PNG/JPEG wallpapers from `/3ds/3hs/backgrounds/`
- Adjustable wallpaper dimming
- Redesigned title details and download interface
- Clear `[A]`, `[B]`, `[X]`, and `[Y]` action badges
- **Size badges** shown next to titles in the browse list
- **Queue reordering** with `[L]`/`[R]` buttons
- Experimental 800px top-screen mode for testing sharper top-screen layouts
- Live download stage, speed, progress, and ETA with speed graph
- Hardware-adaptive performance pipeline:
  - safe Old 3DS/2DS compatibility mode
  - 804 MHz CPU and L2 cache
  - dedicated Core 2 CIA writer on New 3DS
  - pipelined 1 MiB network/install buffers
  - direct CDN connection
- Optional experimental direct-socket CDN transport on New 3DS
- Authenticated network-only CDN benchmark from title details
- Quiet user-initiated download cancellation
- Custom HOME Menu icon and banner
- Fork-safe tracking of official 3hs releases

## Requirements

- Nintendo 3DS-family system
- Luma3DS custom firmware
- FBI or another CIA installer

New 3DS-family systems use the enhanced 804 MHz/L2/Core 2 path. Old 3DS and Old 2DS systems automatically use a safe compatibility path and should expect lower download speeds.

The experimental direct-socket transport can be selected under **Settings → Performance mode**. Authentication remains on the standard HTTP service, and connection setup falls back automatically when the direct path is unavailable.

## Installation

1. Download the latest CIA from [Releases](../../releases) or install it from the Nocturne UniStore in Universal-Updater.
2. Install it with FBI or Universal-Updater.
3. Launch Nocturne from the HOME Menu.

You can install a newer Nocturne CIA over an existing installation. If HOME Menu artwork remains cached, reboot the console or delete and reinstall the application once.

### Universal-Updater

Nocturne updates are handled through a custom Universal-Updater UniStore. In Universal-Updater, open **Settings -> Select UniStore**, press **+**, then enter or scan this UniStore URL:

<img src="assets/nocturne-unistore-qr.png" alt="Nocturne Universal-Updater UniStore QR" width="320">

```text
https://raw.githubusercontent.com/p0mpurin/just-an-hshop-fork/main/universal-updater/nocturne.unistore
```

After adding it once, use Universal-Updater to install the latest Nocturne release. The store downloads `3hs.cia` from the newest GitHub release marked Latest.

### FBI QR install

Scan this with **FBI → Remote Install → Scan QR Code**:

<img src="assets/nocturne-latest-fbi-qr.png" alt="Nocturne latest FBI QR" width="320">

This QR points to the latest `3hs.cia` release asset.

## Wallpapers

Place `.png`, `.jpg`, or `.jpeg` files in:

```text
/3ds/3hs/backgrounds/
```

Open **Settings → Background image** to choose one, then adjust **Wallpaper dimming** for readability.

## Upstream updates

Nocturne does not install the stock 3hs CIA automatically because doing so would overwrite the fork.

When upstream publishes a new release, its source changes must be merged into Nocturne and a new Nocturne CIA must be built.

## Release checklist

Nocturne updates are published through GitHub Releases and the custom Universal-Updater UniStore. Before publishing a feature or fix release, keep these in sync:

- Bump `VERSION_MAJOR`, `VERSION_MINOR`, `VERSION_PATCH`, and `VERSION_DESC` in `include/update.hh`.
- Update `nocturne-version` to the same plain version string, for example `1.5.24`.
- Update `universal-updater/nocturne.unistore`: bump `storeInfo.revision`, app `version`, and `last_updated`.
- Add release notes to `NOCTURNE_CHANGELOG.md`.
- Build with `perl build.pl --target release`.
- Create a matching tag and GitHub release, for example `v1.5.13`.
- Attach both `3hs.cia` and `nocturne-version` to the GitHub release.
- Mark the newest release as **Latest**.

Universal-Updater installs `3hs.cia` from the latest non-prerelease GitHub release. If the newest release is not marked Latest or its `3hs.cia` asset is missing, users will not receive the correct build.

## Building

The project uses devkitARM, libctru, Citro2D/Citro3D, makerom, bannertool, Perl, and mbedTLS.

`source/hsapi_auth.c` is not included in this repository. It contains the client-auth implementation distributed with the official public 3hs source archive. Builders should supply that upstream file locally; personal hShop credentials are not required. Do not commit the generated/local file.

Configure the release target:

```sh
perl build.pl --target release --configure \
  'release,targets=cia,update_base=https://download2.erista.me/3hs,nocturne_update_base=http://your-update-host/nocturne,nb_base=https://hshop.erista.me/nbapi,cdn_base=http://dl.hshopusercontent.com,site_url=https://hshop.erista.me'
```

Then build:

```sh
perl build.pl --target release
```

The output is `3hs.cia`.

## Credits and license

Nocturne is maintained by **p0mpurin** and is based on 3hs by the hShop development team.

The original project credits and build documentation remain available in [README](README). This fork is distributed under the GNU General Public License v3.0; see [LICENSE](LICENSE).
