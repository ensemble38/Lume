# Lume を Windows から iPhone に導入する

この手順は、GitHub Actions で生成した未署名の実機用 IPA を AltStore Classic と AltServer で無料署名して導入する構成です。Apple ID のパスワードや 2FA コードを GitHub、ログ、スクリプトへ保存しないでください。

## 前提

- iPhone: iOS 18 以降、AltStore Classic 導入済み、デベロッパモード有効
- Windows 10 以降
- Apple 公式サイト版 iTunes と iCloud（Microsoft Store 版は AltServer の互換性問題があるため非推奨）
- AltServer が管理者として起動中
- Windows と iPhone が同じプライベート LAN、または USB 接続

## Windows 側

1. iPhone を USB で接続し、ロックを解除する。
2. iPhone に「このコンピュータを信頼しますか？」が出た場合だけ「信頼」を押し、端末のパスコードを入力する。
3. iTunes を開き、iPhone が表示されることを確認する。
4. iTunes の端末概要で「Wi-Fi 経由でこの iPhone と同期」を有効化し、「適用」を押す。
5. AltServer を管理者として起動する。通知領域にひし形のアイコンが表示されることを確認する。
6. Windows Defender Firewall の確認が出た場合は、プライベートネットワークのみ許可する。
7. Bonjour Service と Apple Mobile Device Service が実行中であることを確認する。

既存の AltStore Classic が AltServer と通信できる場合、再インストールは不要です。通信できない場合は、まず USB 接続、同一 LAN、iTunes の Wi-Fi 同期、AltServer の管理者起動、プライベートネットワーク許可の順に確認します。

## IPA を iPhone へ渡す

`outputs/Lume-iPhone-unsigned.ipa` を iCloud Drive、ローカル SMB、または iPhone の「ファイル」アプリから参照できる場所へコピーします。個人用 M3U、認証情報、非公開 EPG URL は GitHub や公開ストレージへアップロードしないでください。

## AltStore Classic で署名・導入

1. Windows 側で AltServer が起動中で、iPhone が USB 接続または同一 LAN にあることを確認する。
2. iPhone で AltStore Classic を開き、`My Apps` 左上の `+` を押す。
3. 「ファイル」から `Lume-iPhone-unsigned.ipa` を選択する。
4. Apple ID の入力を求められた場合は iPhone/AltStore の画面だけで入力する。
5. 署名・インストール完了後、ホーム画面から Lume を起動する。
6. 起動が拒否される場合は「設定」→「一般」→「VPNとデバイス管理」で自分の Apple ID を信頼する。
7. iOS 16 以降では「設定」→「プライバシーとセキュリティ」→「デベロッパモード」を有効にする。再起動と端末上の確認が必要になる。

無料 Apple ID では通常、同時に有効化できるサイドロードアプリは 3 個、登録済み App ID は 10 個までで、署名は 7 日で失効します。Lume には Widget 拡張が含まれるため、Lume 本体とは別の App ID を使用します。

## IPTV 動作確認

1. Lume で M3U プレイリスト追加画面を開く。
2. 既存のローカル M3U ファイルを選択するか、既存 M3U URL を入力する。
3. `#EXTM3U url-tvg="EPG公開URL"` の自動検出、または設定画面の外部 EPG ソースで既存 XMLTV URL を登録する。
4. 同期完了後、118 チャンネル・15 カテゴリーが表示されることを確認する。
5. 番組表で既存の対応対象 110 チャンネルの現在・次番組を確認し、横・縦スクロールする。
6. 権利を持つチャンネルを一つ再生し、映像と音声を確認する。
7. Multi-View を開き、利用可否と同時再生数を確認する。購入判定は変更せず、Pro 表示が出る場合はその制約を記録する。

`.xml.gz` はソース上の Gzip ストリーミング実装とテスト対象に含まれますが、最終判定はユーザーの既存 EPG を使った実機取り込みで行います。

## エラーの切り分け

- `Could not find AltServer`: AltServer の起動、同一 LAN、USB、Bonjour、Wi-Fi 同期、Firewall のプライベート許可を確認。
- App ID 上限: AltStore Classic の `My Apps` → `View App IDs` で失効日を確認し、空きが戻るまで待つ。
- Entitlement/署名エラー: IPA を再ビルドする前に、AltStore が表示する拡張・Entitlement 名と App ID 数を記録する。
- 起動直後クラッシュ: `Payload/Lume.app/Frameworks` の欠落、実機 arm64、iOS バージョン、Widget 署名を確認する。
- 再生だけ失敗: プレイリスト/EPG ではなく再生 URL、選択エンジン、ネットワーク、コーデックを分離して確認する。

