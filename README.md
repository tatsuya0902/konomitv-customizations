# KonomiTV ローカル改造

[KonomiTV](https://github.com/tsukumijima/KonomiTV) v0.14.1 に対して自宅の録画環境で使っている独自機能のパッチと報告の置き場です。元は [PIX-DT295 調査リポジトリ](https://github.com/tatsuya0902/pix-dt295-investigation) の一部でしたが、量が増えたため分離しました。

## 改造内容

| 機能 | 概要 | 報告 / パッチ |
|---|---|---|
| 初期データ待ちの接続削除修正 | 初期データ待ちが10秒を超えると接続が削除される問題を修正 | [報告](REPORT.md)／[パッチ](fix.patch) |
| 疑似フルスクリーン | 字幕・コメントを維持してプレイヤー全体を拡大。回転時のレイアウト追従を含む | [報告](PSEUDO-FULLSCREEN.md)／[パッチ](pseudo-fullscreen.patch)／[レイアウト追従](pseudo-fullscreen-layout.patch) |
| 追いかけ再生 | 録画中の番組を最初から再生可能に（PCR実測で再生可能尺を更新し、EVENTプレイリストで追随）。録画中カードを開けるようにする CSS 修正を含む | [報告](PLAYBACK.md)／[パッチ](chase-playback.patch)／[カード修正](chase-card-click-fix.patch) |
| シークボタン・再生速度コントロール | 再生ボタンの両脇にシークボタン（内側は設定で変更可能な −10/+30 秒、外側は ±60 秒、長押しで高速シーク）、再生速度 0.75〜2 倍を端末に保存 | [報告](PLAYBACK.md)／[パッチ](seek-controls.patch)／[±60 秒化など](ui-adjustments.patch) |
| EPGStation/Jellyfin 連携 | 削除を EPGStation へ委譲（確認フロー付き）、保護状態の表示（ログアウト中も同じバッジ）と付け外し、Jellyfin への削除連動と同期リトライ | [報告](EPGSTATION-INTEGRATION.md)／[パッチ](epgstation-integration.patch) |
| 前の話・次の話ボタン | 視聴画面から同じシリーズの前後の回へ移動。シリーズ情報がない場合はタイトル（話数表記と括弧注記を除いた番組名）の一致で判定 | [報告](RECORDER-FEATURES.md)／[パッチ](episode-nav.patch) |
| 場面一覧 | シークバー用タイル画像を場面サムネイルとして並べ、クリックでその場面へシーク | [報告](RECORDER-FEATURES.md)／[パッチ](chapters.patch) |
| 盛り上がり表示・ダイジェスト再生 | 過去ログコメントの密度グラフをシークバー上に表示し、盛り上がり区間だけを連続再生。ON/OFF は番組情報パネルから | [報告](RECORDER-FEATURES.md)／[パッチ](excitement.patch) ほか |
| 番組表・予約詳細からの視聴ボタン | 番組表と予約詳細から、放送中の番組を直接視聴画面で開く | [報告](ADDITIONAL-FEATURES.md)／[パッチ](program-watch.patch) |
| ゴミ箱（削除の猶予） | 削除せず一覧から除外し、既定30日（保持期間は環境変数で変更可能）保持して完全削除。復元も可能 | [報告](ADDITIONAL-FEATURES.md)／[パッチ](trash.patch) |
| 追いかけ再生の先端追従 | 先端に追いついたら通知し、止まったら先端へ戻して自動再開 | [報告](ADDITIONAL-FEATURES.md)／[パッチ](chase-live-edge.patch) |
| 次話の自動再生・おやすみタイマー・コメント検索・見どころ・未視聴フィルタ・次回予約・今日のハイライト | レコーダー系・コメント系の追加機能一式（視聴画面と録画一覧） | [報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-extras.patch) |
| 画質プロファイル・画質の保持 | モバイル回線向けプロファイルのトグルと手動で選んだ画質を、ページ遷移や別の録画でも引き継ぐ | [報告](ADDITIONAL-FEATURES.md)／[パッチ](quality-persistence.patch) |
| 視聴状態のサーバー一元化・未視聴バッジ | 再生の最大到達位置をサーバー側で保持して世帯で共有し、一覧カードに未視聴バッジを表示。端末やログイン状態によらず同じ判定になる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](watched-state.patch) |
| ログイン時の設定の自動取り込み | 未同期の端末でログインしたとき、サーバーに保存済みの設定があれば自動で反映し、デバイス間同期を有効化する | [報告](ADDITIONAL-FEATURES.md)／[パッチ](settings-autoload-on-login.patch) |
| テレビ向け D-pad 空間ナビゲーション | リモコンの方向キーで項目間を移動し、決定で開く。再生中の決定キーは再生/一時停止になり、一時停止すると操作モードに入って全画面や設定（画質・速度などパネル内も）も操作できる。テレビ向けラッパーでは全画面ボタンがパネルを隠す全画面表示になる | [報告](ADDITIONAL-FEATURES.md)／[パッチ](tv-spatial-navigation.patch) ほか |
| テレビ操作の改善（スクロール・フラットな録画一覧） | チャンネル一覧など縦に長い一覧を、スクロールしながら最後まで移動できるよう空間ナビの候補判定を修正。縦移動は同じ列の中だけにし、一覧の最後で左サイドバーへ飛ばないようにした。sticky なタブバーに先頭の項目が半分被る問題も補正し、同じ列では近い順に選ぶ・サイドバーへは現在のページの項目から始めるよう調整。「ビデオをみる」はセクション分けのホームではなく録画番組一覧へ直接遷移し、全件をフラットに表示する | [報告](ADDITIONAL-FEATURES.md)／[パッチ](tv-scroll-and-flat-videos.patch)・[パッチ](tv-spatial-navigation-column.patch)・[パッチ](tv-spatial-navigation-occlusion.patch)・[パッチ](tv-nav-nearest-and-sidebar-start.patch) |

改造の経緯や詳細な仕様は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) にまとめています。周辺ソフトウェア（EPGStation、Mirakurun、pix-userland）を含めた索引は [CUSTOM-FEATURES.md](CUSTOM-FEATURES.md) を参照してください。

## 適用方法

パッチは KonomiTV v0.14.1 の素のツリーに対して `git apply` で適用できます。全部入れる場合は以下の順序で適用してください（適用順序は検証済み）。

```bash
for p in fix pseudo-fullscreen epgstation-integration seek-controls chase-playback \
         chase-card-click-fix episode-nav chapters excitement excitement-fix \
         digest-button-panel trash chase-live-edge recorder-extras ui-adjustments \
         watched-state quality-persistence program-watch pseudo-fullscreen-layout digest-padding trash-only-deletion \
         settings-autoload-on-login tv-spatial-navigation tv-spatial-navigation-fix tv-control-mode-fix tv-scroll-and-flat-videos tv-spatial-navigation-column tv-spatial-navigation-occlusion tv-nav-nearest-and-sidebar-start tv-nav-pin-skip-and-sidebar-return tv-nav-defer-sticky-chrome tv-focus-style-soft tv-nav-logo-and-search tv-nav-horizontal-row-overlap tv-player-menu-keys; do
  git apply $p.patch
done
```

- `combined-fixes.patch` / `playback-combined.patch` は初期分の一括版で、上記の一部の合本です。
- 各パッチの SHA-256 は `SHA256SUMS*` にまとめています。
