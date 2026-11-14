# Lume iPhone build report

## Result

- Status: GitHub Actions build and IPA validation succeeded
- Lume release: 3.2.0 (build 38)
- Upstream release commit: `be41f66f584595845dc314782fe4cc64d4311c90`
- Patched build commit: `e253f4d5477e9d811c69d15a41bd593306e1169e`
- Branch: `codex/iphone-v3.2.0-controls`
- Workflow run: <https://github.com/ensemble38/Lume/actions/runs/43536033981>
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

- File: `outputs/Lume-v3.2.0-build38-20261114.ipa`
- Size: 65,115,650 bytes
- SHA-256: `606E1EBCF1FCD0450AD0A884977EA97FCCEB0655F0423B174A0F663FC7AB632F`
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
`outputs/run-43536033981/` and in the workflow artifact
`Lume-v3.2.0-iPhone-unsigned`.

## Installation status

- AltServer 1.8.1 is installed and running on Windows.
- Apple Mobile Device Service and Bonjour are running.
- AltStore Classic 2.3.3 launch was verified on the connected iPhone 16 Pro.
- The new IPA is copied to the Windows desktop as both `Lume-v3.2.0-build38-20261114.ipa` and `Lume-iPhone-unsigned.ipa`.
- Installation and launch of this new 3.2.0 build on the iPhone are not yet verified.
