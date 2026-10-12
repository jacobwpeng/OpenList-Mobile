# Build an unsigned iPhone / iPad IPA

This fork adds `.github/workflows/unsigned-ios.yaml` to the official
OpenList-Mobile application. It builds the official OpenList core and the iOS
app together on a GitHub-hosted macOS runner, without signing credentials.
The app requires iOS / iPadOS 13.0 or later, matching the Go mobile framework.

1. Open **Actions → Build unsigned iOS IPA → Run workflow**.
2. Select the OpenList release tag (the default is `v4.2.6`).
3. Download `OpenList-Mobile-<tag>-unsigned` from the successful run's artifacts.
4. Extract the artifact and sign the `.ipa` with your own certificate.

The artifact contains the IPA and its SHA-256 checksum. Artifacts are retained
for 30 days; run the workflow again when another build is needed.

The packaging step checks that the app supports iPad, that the executable
contains the OpenList core, and that the app has no provisioning profile or
valid app signature. This does not substitute for testing on a physical iPad.

The compiled core is cached so app packaging retries do not need to rebuild it.
The inherited automatic Build and Check Updates workflows are disabled in this
fork; this workflow runs only when explicitly requested.

From the command line:

```sh
gh workflow run unsigned-ios.yaml \
  --repo jacobwpeng/OpenList-Mobile \
  --ref main \
  -f openlist_version=v4.2.6
```
