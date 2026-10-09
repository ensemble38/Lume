# Lume iPhone ビルド・導入レポート

更新日: 2026-10-09 (Asia/Tokyo)

## 固定ソース

- Lume upstream base: `62cc493e9ce8d890d09b1675968a079c356c409d` (`main`、v2.4.0 後の公式コミット)
- Lume build commit: `586d2a69f9f93c1fa37345dea73defbdb9119427`（iOS再生開始時の操作UIを非表示にする最小変更を含む）
- LumeEngine: `7c39cf96ea586d1ee3c0685be0b180e1a4241229`
- LumeRecorder: `15e704befc1d35d9d6aef07a61214460965614a2` (`v0.1.1`、公式 sideload workflow と同じ互換ピン)

Lume と LumeEngine は隣接ディレクトリに配置済みです。Lume プロジェクトが `../LumeRecorder/Kit` もローカル参照するため、公式 LumeRecorder も隣接配置しました。

## 調査結果

- Windows: x64、build 26200
- CPU: AMD Ryzen 9 3950X、x64
- PowerShell: 7.6.6
- Git: 2.39.1.windows.1
- Python: 3.8.10
- GitHub CLI: 2.102.0 を導入済み
- 作業ドライブ C: 調査時空き 21.64 GiB
- Lume deployment target: iOS 18.0
- プロジェクト作成ツール: Xcode 26.4
- Bundle Identifier: `com.bilipp.lume`
- Widget Bundle Identifier: `com.bilipp.lume.LumeWidgets`
- 再生エンジン: KSPlayer、VLCKit、AVPlayer、LumeEngine
- LumeEngine FFmpeg: v0.3.0 release の checksum 固定済み `FFmpeg.xcframework.zip` を使用
- Entitlements: Push、CloudKit、App Group、Widget。未署名 archive は Apple 証明書を要求せず、AltStore が無料 Apple ID で再署名する。
- Lume Pro の購入判定コードは変更していない。上流の公式 `Sideload` 構成は `SIDE_LOAD` を定義し、セルフビルドを全機能有効として扱うため、Multi-View はこのIPAでは利用可能になる設計。App Store の Release 構成では従来どおり StoreKit 購入判定を使用する。

公式 v2.4.0 の公開 sideload IPA を構造比較用にだけ取得し、47,777,296 bytes、SHA-256 `C4F2E4517E1E89B3E46059D3E1234DE5FEF895EA3D45F3B24560555AD82B83A7` を確認しました。これは本タスクの生成成果物としては扱いません。

## Windows / AltServer

- Apple Mobile Device Service: 実行中、自動
- Bonjour Service: 実行中、自動
- iTunes 12.13.11.1: Apple 署名済み公式インストーラーから導入済み
- iCloud: Microsoft Store 版 15.10.39.0 が既存。AltServer 互換性は実機接続時に継続確認する。
- AltServer 1.8.0: 公式 CDN から取得し導入・起動済み
- AltServer インストーラー/実行ファイル: Authenticode 署名なし。公式 CDN の取得元と SHA-256 を記録済み。
- AltServer 自動起動: HKCU Run に設定し読戻し確認済み
- Firewall: `AltServer (Private)` 受信許可を Private プロファイル限定で作成・読戻し確認済み
- iPhone USB: 調査時は Apple iPhone デバイスを未検出

詳細な初期調査は作業ディレクトリの `phase-a-inventory.log`、AltServer MSI ログは `altserver-install.log` に保存しています。

## ビルド構成

`.github/workflows/build-ios-ipa.yml` は以下を実行します。

1. Workflow実行コミットのLumeと、固定SHAのLumeEngine/LumeRecorderを隣接 checkout
2. `macos-26` と latest stable Xcode を使用し、Xcode 26.4 以上を実測検証
3. Metal Toolchain を導入
4. Swift Package 依存を解決
5. `generic/platform=iOS`、`Sideload`、`arm64`、`CODE_SIGNING_ALLOWED=NO` で archive
6. 実際に生成された `.app` を `Payload/` に格納
7. ZIP 展開、Info.plist、Bundle ID、arm64、Framework/Extension、サイズ、SHA-256 を検証
8. IPA と build/resolve/構造検証ログを Artifact として保存

## 完了判定

| 項目 | 状態 | 根拠 |
|---|---|---|
| AltServerインストール済み | 完了 | 1.8.0 の登録を確認 |
| AltServer起動確認済み | 完了 | プロセスと待受ポートを確認 |
| iPhone接続確認済み | 未完了 | USB Apple デバイス未検出 |
| Lumeソース取得済み | 完了 | 固定SHAを記録 |
| GitHub Actionsビルド成功 | 完了 | 修正版 Run `37926612849`、Xcode 26.6、全step成功 |
| IPA生成・構造検証成功 | 完了 | 47,807,968 bytes、SHA-256 `6785EC535CCBFEBA3BC0ADD8A0B036EB7B79E24E90A74181EA554615A5B0CD54` |
| WindowsへのIPA取得成功 | 完了 | `outputs/Lume-iPhone-unsigned.ipa` を取得・再展開検証 |
| AltStore Classicで署名成功 | 未完了 | 実機操作待ち |
| iPhoneにインストール成功 | 未完了 | 実機操作待ち |
| Lume起動成功 | 未完了 | 実機操作待ち |
| M3U取り込み成功 | 未完了 | ユーザーの既存M3Uで実機確認待ち |
| EPG表示成功 | 未完了 | ユーザーの既存XMLTVで実機確認待ち |
| IPTV再生成功 | 未完了 | 実機確認待ち |
| 7日更新運用の準備完了 | 進行中 | 自動起動/Firewallは完了、iPhoneとの通信確認が残る |

## IPA 実体の再検証結果

- GitHub Actions Run: `37926612849`
- Lume build commit: `586d2a69f9f93c1fa37345dea73defbdb9119427`
- Xcode: 26.6 (build 17F113)
- SDK/target: `iphoneos`、`arm64-apple-ios18.0`
- IPA: 47,807,968 bytes
- SHA-256: `6785EC535CCBFEBA3BC0ADD8A0B036EB7B79E24E90A74181EA554615A5B0CD54`
- ZIP展開: 成功
- App: `Payload/Lume.app`
- Info.plist: 読込成功
- Bundle Identifier: `com.bilipp.lume`
- MinimumOSVersion: 18.0
- 実行形式: 64-bit Mach-O、CPU type arm64
- Mach-O platform: 2 (iOS)、iOS Simulator platform 7 は不在
- Framework: 25個。`LumeEngine.framework`、`VLCKit.framework`、FFmpegKit系frameworkを確認
- App Extension: `LumeWidgets.appex` 1個
- `_CodeSignature`: 0個。Apple開発者証明書による署名は残っていない
- GitHub Actionsログ: `outputs/build.log`、依存解決ログ: `outputs/resolve.log`

## iOSプレーヤー操作UI修正

ユーザー確認で、受信開始後も一時停止・チャンネル切替ボタンが半透明表示のまま残る問題が判明しました。KSPlayer、VLCKit、AVPlayer、LumeEngine の4ホストすべてで、iOSだけ操作UIの初期状態を非表示へ変更しました。画面タップで表示し、再生中は既存の4秒タイマーで再度非表示になります。tvOS/macOS/visionOSの初期表示は変更していません。修正版をRun `37926612849` で再ビルドし、生成物を再検証しました。
