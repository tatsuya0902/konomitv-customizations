# KonomiTV ローカル改造

[KonomiTV](https://github.com/tsukumijima/KonomiTV) v0.14.1 に対して自宅の録画環境で使っている独自機能のパッチと報告の置き場です。元は [PIX-DT295 調査リポジトリ](https://github.com/tatsuya0902/pix-dt295-investigation) の一部でしたが、量が増えたため分離しました。

## 改造内容

| 機能 | 概要 | 報告 / パッチ |
|---|---|---|
| 初期データ待ちの接続削除修正 | 初期データ待ちが10秒を超えると接続が削除される問題を修正 | [報告](REPORT.md)／[パッチ](base-integration.patch) |
| 疑似フルスクリーン | 字幕・コメントを維持してプレイヤー全体を拡大。回転時のレイアウト追従を含む | [報告](PSEUDO-FULLSCREEN.md)／[パッチ](base-integration.patch)／[レイアウト追従](refinements.patch) |
| 追いかけ再生 | 録画中の番組を最初から再生可能に（PCR実測で再生可能尺を更新し、EVENTプレイリストで追随）。録画中カードを開けるようにする CSS 修正を含む | [報告](PLAYBACK.md)／[パッチ](playback.patch)／[カード修正](playback.patch) |
| シークボタン・再生速度コントロール | 再生ボタンの両脇にシークボタン（内側は設定で変更可能な −10/+30 秒、外側は ±60 秒、長押しで高速シーク）、再生速度 0.75〜2 倍を端末に保存 | [報告](PLAYBACK.md)／[パッチ](playback.patch)／[±60 秒化など](recorder-features.patch) |
| スマホ縦画面でのプレイヤーボタン配置 | スマホ縦画面ではコントロールバーの ±60 秒スキップボタンを隠し、アイコンの間隔を詰めて、チャプターボタン・キャプチャボタン・プレイヤー再起動ボタンなどが重ならないようにする | [パッチ](mobile-player-controls.patch) |
| 「一部のみ」判定の許容誤差 | 録画開始・終了の数秒のずれでは「一部のみ（部分録画）」フラグを立てず、10 秒を超えたずれだけを部分録画とみなす。正常に録画できた番組への誤ったフラグを防ぐ。あわせて表示文言も「一部のみ」から「録画不完全」に変更 | [パッチ](partially-recorded-tolerance.patch) |
| 続き再生の巻き戻し | 前回視聴した続きから再生するとき、保存位置より少し手前（既定10秒、設定で0/5/10/15/30秒に変更可）から再開する。端末ローカル設定 | [報告](PLAYBACK.md)／[パッチ](playback.patch) |
| EPGStation/Jellyfin 連携 | 削除を EPGStation へ委譲（確認フロー付き）、保護状態の表示（ログアウト中も同じバッジ）と付け外し、Jellyfin への削除連動と同期リトライ | [報告](EPGSTATION-INTEGRATION.md)／[パッチ](base-integration.patch) |
| 前の話・次の話ボタン | 視聴画面から同じシリーズの前後の回へ移動。シリーズ情報がない場合はタイトル（話数表記と括弧注記を除いた番組名）の一致で判定 | [報告](RECORDER-FEATURES.md)／[パッチ](playback.patch) |
| 場面一覧 | シークバー用タイル画像を場面サムネイルとして並べ、クリックでその場面へシーク | [報告](RECORDER-FEATURES.md)／[パッチ](comment-highlights.patch) |
| しおり | 再生中に「しおりを追加」で現在位置を保存し、場面一覧ダイアログからジャンプ・削除できる。サーバー側で世帯共有し、録画の削除時に一緒に消える | [報告](RECORDER-FEATURES.md)／[パッチ](recorder-features.patch) |
| 盛り上がり表示・ダイジェスト再生 | 過去ログコメントの密度グラフをシークバー上に表示し、盛り上がり区間だけを連続再生。ON/OFF は番組情報パネルから | [報告](RECORDER-FEATURES.md)／[パッチ](comment-highlights.patch) ほか |
| 番組表・予約詳細からの視聴ボタン | 番組表と予約詳細から、放送中の番組を直接視聴画面で開く | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| ゴミ箱（削除の猶予） | 削除せず一覧から除外し、既定30日（保持期間は環境変数で変更可能）保持して完全削除。復元も可能 | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| 追いかけ再生の先端追従 | 先端に追いついたら通知し、止まったら先端へ戻して自動再開 | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| 次話の自動再生・おやすみタイマー・コメント検索・見どころ・未視聴フィルタ・次回予約・今日のハイライト | レコーダー系・コメント系の追加機能一式（視聴画面と録画一覧） | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| 画質プロファイル・画質の保持 | モバイル回線向けプロファイルのトグルと手動で選んだ画質を、ページ遷移や別の録画でも引き継ぐ | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| 視聴状態のサーバー一元化・未視聴バッジ | 再生の最大到達位置をサーバー側で保持して世帯で共有し、一覧カードに未視聴バッジを表示。端末やログイン状態によらず同じ判定になる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-features.patch) |
| 録画結果一覧 | 予約録画の結果（録画できた／一部のみ／失敗）を一覧表示。EPGStation は失敗を残さないため、KonomiTV 側で5分ごとに予約と録画を照合して記録する（尺の割合とドロップ数で判定） | [報告](RECORDER-FEATURES.md)／[パッチ](recorder-features.patch) |
| シリーズ予約（キーワード自動予約） | 視聴画面パネルと予約詳細の「シリーズ予約」から、番組名（話数表記を除去）＋同じチャンネルの自動予約条件を作成し、以後は毎回自動で予約される。条件の一覧・削除は録画予約一覧の「自動予約の条件」から | [報告](RECORDER-FEATURES.md)／[パッチ](recorder-features.patch) |
| 録画予約まわりの導線改善 | 「録画予約」を開くと直接「録画予約一覧」を表示（従来の「放送が近い録画予約」画面は廃止）。手動予約の無効化は EPGStation に対応する状態がないため、保存時に案内を出す | [パッチ](recorder-features.patch) |
| CM区間の受け口（チャプター対応） | 録画に CM 区間情報がある場合、場面一覧にCM区間を表示し、プレイヤーに前/次チャプターボタンを追加（レコーダー同様にCMの境目を移動）。CM区間は録画ファイル横の .chapter.txt から本体が取り込む仕組みを利用。検出エンジン本体は別リポジトリの [cm-detect](https://github.com/tatsuya0902/cm-detect)（AGPL-3.0）が .chapter.txt を生成する | [パッチ](recorder-features.patch) |
| CM自動スキップ＋シークバー表示 | 設定で有効にすると、再生が CM 区間へ入ったときに自動で CM の終わりまでスキップ（端末ごとに設定、既定は無効）。シークバーには CM 区間が帯状に表示される | [パッチ](recorder-features.patch) |
| ライブ視聴のチューナー連動 | 空きチューナーがないとき、受信中でもないチャンネルをチャンネル一覧（テレビをみる／視聴画面パネル）でグレー表示にし、「チューナー使用中」と表示して視聴できないようにする。録画中など受信中のチャンネル（物理チャンネル単位）は、チューナーを共有してそのまま視聴できる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](live-tuner-gating.patch) |
| 録画容量の表示 | 「ビデオをみる」のトップに、録画ファイルの合計サイズと保存先ストレージの全体・空き容量を表示する。録画がどれだけ容量を使っているかが KonomiTV の画面から分かる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](storage-display.patch) |
| ログイン時の設定の自動取り込み | 未同期の端末でログインしたとき、サーバーに保存済みの設定があれば自動で反映し、デバイス間同期を有効化する | [報告](ADDITIONAL-FEATURES.md)／[パッチ](refinements.patch) |
| テレビ向け D-pad 空間ナビゲーション | リモコンの方向キーで項目間を移動し、決定で開く。再生中の決定キーは再生/一時停止になり、一時停止すると操作モードに入って全画面や設定（画質・速度などパネル内も）も操作できる。テレビ向けラッパーでは全画面ボタンがパネルを隠す全画面表示になる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](tv-support.patch) |
| テレビ操作の改善（スクロール・フラットな録画一覧） | チャンネル一覧など縦に長い一覧を、スクロールしながら最後まで移動できるよう空間ナビの候補判定を修正。縦移動は同じ列の中だけにし、一覧の最後で左サイドバーへ飛ばないようにした。sticky なタブバーに先頭の項目が半分被る問題も補正し、同じ列では近い順に選ぶ・サイドバーへは現在のページの項目から始めるよう調整。「ビデオをみる」はセクション分けのホームではなく録画番組一覧へ直接遷移し、全件をフラットに表示する | [報告](ADDITIONAL-FEATURES.md)／[パッチ](tv-support.patch) |
| リモコンのメディアキー（前/次チャプター） | テレビのリモコンの「前」「次」ボタンで、CM区間の境目をチャプター移動できる（MediaSession の previoustrack/nexttrack と MediaTrackNext/Previous キーに対応）。CM区間がない録画では無効で、ラッパーアプリ側は従来の話数移動にフォールバックする | [報告](ADDITIONAL-FEATURES.md)／[パッチ](tv-support.patch) |
| EPGStation管理外録画のローカル保護とローカル削除 | EPGStation の管理外の録画（BDZ から取り込んだもの等）も、KonomiTV ローカルの保護フラグで保護できる。保護中はゴミ箱移動と自動消去の対象外。ゴミ箱からの完全削除と自動消去にも、管理外の場合のローカル削除経路を追加（削除は確認フロー付き、HMAC トークンで認証） | [報告](RECORDER-FEATURES.md)／[パッチ](base-integration.patch)／[パッチ](recorder-features.patch) |
| 「保護中のみ」フィルタと実際の録画時間表示 | 録画番組一覧に「保護中のみ」チップを追加（「未視聴のみ」との併用は AND）。一覧の時刻行は番組時間ではなく実際の録画時間（録画開始〜終了、秒なし）を表示し、録画期間がない場合は番組時間にフォールバックする | [報告](RECORDER-FEATURES.md)／[パッチ](filter-ui.patch) |
| フィルタメニューとアイコンオンリー化 | スマホ縦ではフィルタ類を「フィルタ」メニューに集約（未視聴のみ／保護中のみ／今日のハイライト＋並び替え）。ON 件数バッジ付きで、ON 時はピンク。360px幅ではアイコンオンリーになる。デスクトップ／テレビはチップのまま | [報告](RECORDER-FEATURES.md)／[パッチ](filter-ui.patch) |

改造の経緯や詳細な仕様は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) にまとめています。周辺ソフトウェア（EPGStation、Mirakurun、pix-userland）を含めた索引は [CUSTOM-FEATURES.md](CUSTOM-FEATURES.md) を参照してください。

## 適用方法

パッチは KonomiTV v0.14.1 の素のツリーに対して `git apply` で適用できます。全部入れる場合は以下の順序で適用してください（適用順序は検証済み）。

```bash
for p in base-integration playback comment-highlights recorder-features refinements tv-support storage-display live-tuner-gating filter-ui mobile-player-controls partially-recorded-tolerance; do
  git apply $p.patch
done
```

- `combined-fixes.patch` / `playback-combined.patch` は初期分の一括版で、上記の一部の合本です。
- 各パッチの SHA-256 は `SHA256SUMS*` にまとめています。
