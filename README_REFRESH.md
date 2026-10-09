# AltStore Classic の 7 日署名更新

署名更新は既存 IPA の再署名であり、Lume のソースに変更がなければ IPA の再ビルドは不要です。

## 通常の更新

1. Windows を起動し、AltServer が通知領域で実行中であることを確認する。
2. iPhone と Windows を同じプライベート LAN に接続する。失敗時は USB 接続する。
3. iPhone で AltStore Classic → `My Apps` → `Refresh All` を押す。
4. Lume と AltStore の残り日数が 7 日近くへ戻ったことを画面で確認する。

期限切れ前に定期的に AltStore を開くと、Background Refresh が AltServer を検出しやすくなります。

## 失敗時の回復順

1. AltServer を管理者として再起動する。
2. iPhone のロック解除、USB 接続、信頼状態を確認する。
3. iTunes で端末が見え、Wi-Fi 同期が有効なことを確認する。
4. Bonjour Service と Apple Mobile Device Service を再確認する。
5. Windows と iPhone が同一 LAN か確認し、必要なら USB 更新へ切り替える。
6. AltStore Classic の `View App IDs` で上限と失効日を確認する。
7. Apple ID セッション切れの場合のみ、AltStore/AltServer の正規画面で再認証する。認証情報をログやスクリプトへ保存しない。

AltStore Classic の非アクティブ化はアプリデータをバックアップして枠を空け、再アクティブ化時にデータを復元する機能です。単なる更新失敗で Lume や AltStore Classic を削除しないでください。

## Lume の更新

Lume または依存関係を更新するときだけ `.github/workflows/build-ios-ipa.yml` の固定 SHA を互換バージョンへ変更し、GitHub Actions を再実行します。新しい IPA は既存の Bundle Identifier を維持し、AltStore Classic から上書き導入します。上書き前にアプリ内データが保持されることを確認し、削除→再導入は最後の手段にします。

