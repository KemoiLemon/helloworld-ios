# HelloWorld (unsigned IPA)

Minimal SwiftUI app that shows "Hello, World!", built into an **unsigned IPA** by GitHub Actions so it can be sideloaded with [LiveContainer](https://github.com/LiveContainer/LiveContainer).

## Get the IPA

Push this repo to GitHub, then either:

- **Newest build, fixed URL** — Releases page → release **latest** → `HelloWorld.ipa`, or download it directly:

  ```
  curl -fL -o HelloWorld.ipa https://github.com/<owner>/<repo>/releases/latest/download/HelloWorld.ipa
  ```

- **A specific build** — every push also creates its own release tagged with the full commit SHA (titled with the short SHA). Those never change once published.

The `latest` release is rebuilt from scratch on each push and re-flagged as GitHub's latest release; the commit releases stay as history. You can also trigger a build manually via Actions → *Build unsigned IPA* → *Run workflow*.

The build runs:

```
xcodebuild -project HelloWorld.xcodeproj -target HelloWorld \
  -configuration Release -sdk iphoneos \
  CODE_SIGNING_ALLOWED=NO CODE_SIGNING_REQUIRED=NO CODE_SIGN_IDENTITY="" build
```

then wraps `HelloWorld.app` in `Payload/` and zips it as `HelloWorld.ipa`. No certificates, no Apple account, no signing step.

## Install with LiveContainer

1. Open LiveContainer → **+** → pick `HelloWorld.ipa`.
2. Launch it from the app list. LiveContainer signs the binary itself, so the IPA must stay unsigned.

Requirements/notes:

- Deploys to **iOS 16+**, device (arm64) build, bundle id `com.example.helloworld`.
- The app has no entitlements, no App Groups and no extensions — the things LiveContainer cannot fake. Keep it that way if you extend it.
- LiveContainer itself must already be installed and signed with a sideloading tool (AltStore/SideStore, etc.).

## Build locally

Open `HelloWorld.xcodeproj` in Xcode 14+ and run on a simulator, or use the same `xcodebuild` command above on a Mac.
