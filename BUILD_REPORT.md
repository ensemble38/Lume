# Lume iPhone build report

## Result

- Status: GitHub Actions build and IPA validation succeeded
- Lume release: 3.1.0 (build 37)
- Upstream release commit: `b50d0ba5007f1807aae5b633c5687ee5cf9b2ae8`
- Patched build commit: `cde788ce72fe763c7e56d162a7a6548ad9384300`
- Branch: `codex/iphone-v3.1.0-controls`
- Workflow run: <https://github.com/ensemble38/Lume/actions/runs/43214526045>
- Runner: `xcode-27`
- Xcode: 27.2 (27B52)
- Configuration: Sideload / Release / iphoneos / arm64

## Applied changes

1. KSPlayer, VLC, AVPlayer, LumeEngine and AetherEngine players now start with controls hidden on iOS.
2. A tap reveals the controls; the existing four-second dismissal timer hides them again.
3. The sideload build uses a bundle-specific background download session identifier to avoid a stale-session collision at launch.
4. No purchase or Lume Pro checks were changed.

## Dependency revisions

- LumeEngine: `v0.5.0`
- LumeRecorder: `v0.1.1`
- LumeCloud: `2180a86d9f589dce0cbe4edfe4f261f8fcede3ba`
- AetherEngine: project-pinned `7.59.0`

## IPA validation

- File: `outputs/Lume-iPhone-unsigned.ipa`
- Size: 64,823,796 bytes
- SHA-256: `0FBDBE24598270DB694389928C14771C6CA130D5D3C4A6639BCB716107AF614F`
- ZIP integrity: passed
- App: `Payload/Lume.app`
- Bundle identifier: `com.bilipp.lume`
- Executable: `Lume`
- Architecture: arm64
- Minimum iOS: 18.0
- Frameworks: 36 embedded frameworks found
- Extension: `LumeWidgets.appex`
- Embedded provisioning profile: absent
- Signing: app intentionally unsigned for AltServer/AltStore re-signing

The validation report and complete build/package logs are in
`outputs/run-43214526045/` and in the workflow artifact
`Lume-v3.1.0-iPhone-unsigned`.

## Installation status

- AltServer 1.8.1 is installed and running on Windows.
- Apple Mobile Device Service and Bonjour are running.
- The IPA is copied to the Windows desktop as `Lume-iPhone-unsigned.ipa`.
- Device installation and launch verification require the iPhone to be connected and unlocked, followed by the user's Apple ID authentication in AltServer.

