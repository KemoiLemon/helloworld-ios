# HelloWorld (unsigned IPA)

Minimal SwiftUI app that shows "Hello, World!", built into an **unsigned IPA** by GitHub Actions so it can be sideloaded with [LiveContainer](https://github.com/LiveContainer/LiveContainer).

## Get the IPA

1. Push this repo to GitHub.
2. Actions → **Build unsigned IPA** → wait for the run.
3. Download the `HelloWorld-unsigned` artifact and unzip it — you get `HelloWorld.ipa`.

Every push to any branch rebuilds it; you can also run it manually via *Run workflow*.

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
