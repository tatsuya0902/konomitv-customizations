# 独自機能一覧（ソフトウェア編）

[PIX-DT295 調査リポジトリ](https://github.com/tatsuya0902/pix-dt295-investigation)から分離した、録画環境（pix-userland ＋ Mirakurun ＋ EPGStation ＋ KonomiTV）の周辺ソフトウェアの独自機能をまとめた索引です。各機能の詳細と適用方法は、リンク先の報告を参照してください。

KonomiTV への改造の全記録（未公開分を含む。アップデート時や再構築時の復元用）は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) にまとめています。

## KonomiTV（適用基準：v0.14.1）

| 機能 | 概要 | 状態 |
|---|---|---|
| 初期データ待ちの接続削除修正 | 初期データ待ちが10秒を超えると接続が削除される問題を修正 | 公開済み：[報告](REPORT.md)／[パッチ](fix.patch) |
| 疑似フルスクリーン | 字幕・コメントを維持してプレイヤー全体を拡大 | 公開済み：[報告](PSEUDO-FULLSCREEN.md)／[パッチ](pseudo-fullscreen.patch)／[回転レイアウト追従](pseudo-fullscreen-layout.patch) |
| 追いかけ再生 | 録画中の番組を最初から再生可能に（PCR実測で再生可能尺を更新し、EVENTプレイリストで追随）。録画中カードを開けるようにする CSS 修正を含む | 公開済み：[報告](PLAYBACK.md)／[パッチ](chase-playback.patch)／[カード修正](chase-card-click-fix.patch) |
| シークボタン・再生速度コントロール | 再生ボタンの両脇にシークボタン（内側は設定で変更可能な −10/+30 秒、外側は ±60 秒、長押しで高速シーク）、再生速度 0.75〜2 倍を端末に保存 | 公開済み：[報告](PLAYBACK.md)／[パッチ](seek-controls.patch)／[±60 秒化など](ui-adjustments.patch) |
| EPGStation/Jellyfin 連携 | 削除を EPGStation へ委譲（確認フロー付き）、保護状態の表示（ログアウト中も同じバッジ）と付け外し、Jellyfin への削除連動と同期リトライ | 公開済み：[報告](EPGSTATION-INTEGRATION.md)／[パッチ](epgstation-integration.patch) |
| 前の話・次の話ボタン | 視聴画面から同じシリーズの前後の回へ移動。シリーズ情報がない場合はタイトル（話数表記と括弧注記を除いた番組名）の一致で判定 | 公開済み：[報告](RECORDER-FEATURES.md)／[パッチ](episode-nav.patch) |
| 場面一覧 | シークバー用タイル画像を場面サムネイルとして並べ、クリックでその場面へシーク | 公開済み：[報告](RECORDER-FEATURES.md)／[パッチ](chapters.patch) |
| 盛り上がり表示・ダイジェスト再生 | 過去ログコメントの密度グラフをシークバー上に表示し、盛り上がり区間だけを連続再生。ON/OFF は番組情報パネルから | 公開済み：[報告](RECORDER-FEATURES.md)／[パッチ](excitement.patch) ほか |
| 番組表・予約詳細からの視聴ボタン | 番組表と予約詳細から直接視聴画面を開く | 公開済み：[報告](ADDITIONAL-FEATURES.md)／[パッチ](program-watch.patch) |
| ゴミ箱（削除の猶予） | 削除せず一覧から除外し、7日後に完全削除。復元も可能 | 公開済み：[報告](ADDITIONAL-FEATURES.md)／[パッチ](trash.patch) |
| 追いかけ再生の先端追従 | 先端に追いついたら通知し、止まったら先端へ戻して自動再開 | 公開済み：[報告](ADDITIONAL-FEATURES.md)／[パッチ](chase-live-edge.patch) |
| 次話の自動再生・おやすみタイマー・コメント検索・見どころ・未視聴フィルタ・次回予約・今日のハイライト | 2026-11-13 追加のレコーダー系・コメント系機能一式（視聴画面と録画一覧） | 公開済み：[報告](ADDITIONAL-FEATURES.md)／[パッチ](recorder-extras.patch) |
| 画質プロファイル・画質の保持 | モバイル回線向けプロファイルのトグルと手動で選んだ画質を、ページ遷移や別の録画でも引き継ぐ | 公開済み：[報告](ADDITIONAL-FEATURES.md)／[パッチ](quality-persistence.patch) |
| 視聴状態のサーバー一元化 | 未視聴判定を端末ごとではなく世帯全体で共有 | 非公開（自宅専用。内容は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) に記載） |

## EPGStation

単体への改造はありません。上記の KonomiTV 連携の相手先として使用しており、削除・保護の管理元は EPGStation 側に置いています（KonomiTV からは EPGStation の正式 API 経由で委譲）。

## Mirakurun と運用ノウハウ

パッチ形式の公開物はありませんが、自環境では次の構成で運用しています。詳細は[ブログ](https://blog.tatsuya0902.jp/)の備忘録に記録しています。

- Mirakurun の2インスタンス構成：PIX-DT295 用のユーザーバックエンド（pix-userland 連携）と、EPGStation / KonomiTV が参照する Docker 版フロント
- BS / CS（110度）および有料 BS の受信有効化：無料放送デーに備え、実測で物理スロットを特定して両インスタンスに登録

## pix-userland（ドライバ）

受信停止の調査と修正候補は本題側にまとめています：[報告](https://github.com/tatsuya0902/pix-dt295-investigation/blob/main/pix-userland/REPORT.md)／[パッチ](https://github.com/tatsuya0902/pix-dt295-investigation/blob/main/pix-userland/fix.patch)
