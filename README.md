# special-guide

A GitHub Actions–based build mirror for [termux/termux-app](https://github.com/termux/termux-app).

## What this does

The `Build` workflow (`.github/workflows/build.yml`) checks out the upstream
termux-app source, optionally rebrands the app display name and package
identifier, and then compiles **both** package variants
(`apt-android-7` and `apt-android-5`) using the same debug signing certificate
that is bundled in the termux-app repository.

Five per-ABI APKs (universal, arm64-v8a, armeabi-v7a, x86_64, x86) plus a
`sha256sums` file are uploaded as workflow artifacts after every successful run.

## Triggering a build

### Automatic

Pushes to `master` / `main` and pull requests against those branches trigger
the workflow automatically.

### Manual (custom name)

Go to **Actions → Build → Run workflow** and fill in the optional inputs:

| Input | Default | Description |
|-------|---------|-------------|
| `app_name` | `Termux` | Display name shown on the device home screen |
| `app_package` | `com.termux` | Android package/namespace identifier |

Leaving either field blank keeps the upstream default.

## Certificates

Builds are signed with the debug keystore that is already committed in the
upstream termux-app repository (`app/testkey_untrusted.jks`).  All builds
produced by this workflow share the same signing certificate regardless of the
chosen app name.