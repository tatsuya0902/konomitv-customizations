# KonomiTV レコーダー系表示機能パッチ (v0.14.1 向け)

市販レコーダーに近い視聴体験のための、表示・移動まわりの機能追加です。
いずれも KonomiTV v0.14.1 に対する差分パッチで、自環境 (本番) では 2026-11-13 に反映済みです。

## 含まれる機能とパッチ

| 機能 | パッチ | 内容 |
|---|---|---|
| 前の話・次の話ボタン | `episode-nav.patch` | 単体取得 API に `next_episode_id` / `previous_episode_id` を追加し、視聴画面の番組情報パネルに「前の話」「次の話」ボタンを表示する。シリーズ ID が全録画で空の環境でも動くよう、タイトルから話数表記と括弧注記を除いた番組名の一致でも判定する (前後とも録画として存在する回にだけ移動する。欠けた回には飛ばない) |
| 場面一覧 | `chapters.patch` | 番組情報パネルに「場面一覧」ボタンを追加。シークバー用タイル画像を約12場面に間引いてグリッド表示し、クリックでその場面へシークするダイアログ (ChapterListDialog) |
| 盛り上がり表示とダイジェスト再生 | `excitement.patch` / `excitement-fix.patch` / `digest-button-panel.patch` | 過去ログコメントを5秒ビンで集計し、シークバーの上に密度グラフを表示 (クリックでシーク)。密度が高い区間だけを連続再生するダイジェスト再生。`excitement-fix` は帯の配置をシークバー内側へ直す修正、`digest-button-panel` はダイジェストの ON/OFF ボタンをプレイヤー内から番組情報パネルへ移す変更 (プレイヤー内の小さいボタンは押しにくいため) |
| 追いかけ再生のカード修正 | `chase-card-click-fix.patch` | 録画中カード全体に掛かる上流 CSS の `pointer-events: none` を録画中カードだけ外し、追いかけ再生のためにカードを開けるようにする修正 (解析失敗カードは従来通り無効)。`chase-playback.patch` の利用者向け |
| しおり | `recorder-features.patch` | 視聴画面の番組情報パネルに「しおりを追加」ボタン。現在位置をサーバー側 (server/data/bookmarks.json) に保存して世帯で共有し、場面一覧ダイアログの「しおり」欄からジャンプ・削除できる。録画の削除時に一緒に消える |
| 録画結果一覧 | `recorder-features.patch` | 「ビデオをみる」のトップに「録画結果」ボタン。EPGStation の DB を5分ごとに読み取り専用で照合し、予約の結果 (録画できた / 一部のみ / 失敗) を server/data/recording-results.json に記録して表示する。判定は尺が予定の95%以上かと録画ファイルの有無。ドロップ数も併記する |
| シリーズ予約（キーワード自動予約） | `recorder-features.patch` | 視聴画面の番組情報パネル（「次回を予約」の隣）と予約詳細ドロワーに「シリーズ予約」ボタン。番組名から話数表記などを除いたキーワード＋同じチャンネルの自動予約条件 (EDCB AutoAdd → EPGStation ルール) を作成し、以後は自動で予約される。同じキーワード＋チャンネルの条件がある場合は作成しない。録画予約一覧の「自動予約の条件」ボタンから条件の一覧表示と削除ができる (条件を消しても作成済みの予約は残る) |
| 録画予約まわりの導線改善 | `recorder-features.patch` | /reservations/ で「放送が近い録画予約」を挟まず直接「録画予約一覧」を表示 (/reservations/all はリダイレクト)。手動予約の無効化は EPGStation に対応する状態がなくブリッジが 500 を返すため、手動予約では「録画予約の有効/無効」トグル自体を表示しない (録画設定フォーム・録画予約一覧とも、自動予約の予約でのみ表示し、手動予約は「有効」の固定表示)。保険として保存時にもガードし、案内を出して保存しない。ゴミ箱には「自動予約の条件」と同じピル型の「録画一覧に戻る」ボタンを追加 |
| CM区間の受け口（チャプター対応） | `recorder-features.patch` | 本体には録画ファイル横の .chapter.txt（AviUtl 形式、名前が CM で始まるチャプターを CM 区間とする）を取り込む CMSectionsDetector と cm_sections の配信があるが、再生側で使う機能がなかった。場面一覧ダイアログに「CM区間」セクションを追加 (クリックで開始位置へシーク)、プレイヤーのコントロールバーに前/次チャプターボタンを追加し、CM 区間の境目をレコーダーのチャプターボタンと同様に移動できるようにした (区間がない録画ではボタンを出さない)。検出エンジン側はこの受け口に .chapter.txt を渡す形で別途連携する（検出エンジン本体：[cm-detect](https://github.com/tatsuya0902/cm-detect)、denpa の判定コアを切り出した別ツールで AGPL-3.0） |
| CM自動スキップ＋シークバー表示 | `recorder-features.patch` | 設定 (一般 → CM自動スキップ、端末ローカル・既定 OFF) を有効にすると、再生が CM 区間の開始を跨いだときに自動で区間の終わりへシークする (CmAutoSkipManager が timeupdate で監視。シークで区間の途中へ入った場合は発動しない)。あわせてシークバー上に CM 区間を暗い帯で表示する |
| ローカル保護とローカル削除（EPGStation管理外） | `base-integration.patch` / `recorder-features.patch` | EPGStation の管理外の録画（BDZ から取り込んだもの等）は、KonomiTV ローカルの保護フラグ（`server/data/recording-local-protection.json`）で保護する。保護中はゴミ箱移動と自動消去の対象外。ゴミ箱からの完全削除と自動消去（`purgeExpired()`）にも、管理外の場合のローカル削除経路を追加（HMAC トークンによる確認フロー付き）。振り分けは EPGStation 照合の件数で決める：1件一致 → EPGStation 経由、0件 → ローカル経路、複数件 → ローカル削除はしない。管理下の保護・解除の挙動は変わらない |
| 「保護中のみ」フィルタと実際の録画時間表示 | `filter-ui.patch` | 録画番組一覧に「保護中のみ」チップを追加（「未視聴のみ」との併用は AND、判定は `is_protected === true`）。一覧の時刻行は番組時間ではなく実際の録画時間（録画開始〜終了、秒なし）を表示し、録画期間がない場合は番組時間にフォールバックする。サーバー側の変更は不要。カードは全画面で共通のため Programs／Home／Search／Mylist／WatchedHistory で表示が変わる |
| フィルタメニューとアイコンオンリー化 | `filter-ui.patch` | スマホ縦ではフィルタ類を「フィルタ」メニューに集約（未視聴のみ／保護中のみ／今日のハイライト＋並び替え「新しい順／古い順」）。ON 件数バッジ付きで ON 時はピンク。360px幅では既存の `smartphone-vertical-short`（380px以下・縦向き）でアイコンオンリーになる（ON 件数バッジは残る）。デスクトップ／テレビはチップのまま。並び替えは基本「新しい順」から動かさないため、メニュー化で2タップになっても実害はほぼない |

## 適用方法

素の v0.14.1 に対して、次の順序で適用してください (後段のパッチは前段のパッチ適用済みの状態を前提にしています)。

```bash
# 先に公開済みの再生系パッチを適用する (excitement.patch の PlayerController 側の文脈が seek-controls に依存します)
git apply seek-controls.patch
git apply chase-playback.patch

# 本セット
git apply chase-card-click-fix.patch
git apply episode-nav.patch
git apply chapters.patch
git apply excitement.patch
git apply excitement-fix.patch
git apply digest-button-panel.patch
```

- `episode-nav.patch` はサーバー側 (schemas / VideosRouter) の変更を含みます。サーバーを再起動してください。
- それ以外はクライアントのみの変更です。再ビルド (`yarn build` 相当) で反映されます。
- ハッシュの照合には `SHA256SUMS-recorder` を使ってください。

## 検証状況 (2026-11-13 時点、正直な現状)

- 全パッチは素の v0.14.1 への上記順序での `git apply` を確認済みです。
- 場面一覧: 利用者が実機でボタンの表示と場面へのジャンプを確認済み。
- 盛り上がり表示: コメント4,186件の大相撲録画でグラフ表示を実機スクリーンショットで確認済み。表示位置の重なりは `excitement-fix` で修正済み。
- ダイジェスト再生: 区間スキップの連続再生そのものは、利用者による明示的な動作確認はまだありません。
- 前の話・次の話: サーバー側の判定ロジックは実データで検証済み (前後の回・解析失敗回のスキップ・別番組への誤判定なし)。ボタンのクリックは利用者未確認です。
- 追いかけ再生のカード修正: 修正後、利用者が実機で追いかけ再生できることを確認済み。
- 2026-11-15 追加分（ローカル保護・「保護中のみ」フィルタ・実際の録画時間表示・フィルタメニュー・アイコンオンリー化）は本番に反映済みで、素の v0.14.1 に9バンドル（base-integration、playback、comment-highlights、recorder-features、refinements、tv-support、storage-display、live-tuner-gating、filter-ui）を順に適用したツリーが本番ツリーと一致することを確認済み。詳細は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) を参照。
