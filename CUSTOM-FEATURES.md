# 独自機能一覧（ソフトウェア編）

[PIX-DT295 調査リポジトリ](https://github.com/tatsuya0902/pix-dt295-investigation)から分離した、録画環境（pix-userland ＋ Mirakurun ＋ EPGStation ＋ KonomiTV）の周辺ソフトウェアの独自機能をまとめた索引です。

KonomiTV への改造の一覧は [README](README.md#改造内容) を参照してください。改造の経緯や詳細な仕様は [LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md) にまとめています。

## EPGStation

単体への改造はありません。KonomiTV 連携の相手先として使用しており、削除・保護の管理元は EPGStation 側に置いています（KonomiTV からは EPGStation の正式 API 経由で委譲）。

## Mirakurun と運用ノウハウ

パッチ形式の公開物はありませんが、自環境では次の構成で運用しています。詳細は[ブログ](https://blog.tatsuya0902.jp/)の備忘録に記録しています。

- Mirakurun の2インスタンス構成：PIX-DT295 用のユーザーバックエンド（pix-userland 連携）と、EPGStation / KonomiTV が参照する Docker 版フロント
- BS / CS（110度）および有料 BS の受信有効化：無料放送デーに備え、実測で物理スロットを特定して両インスタンスに登録

## pix-userland（ドライバ）

受信停止の調査と修正候補は本題側にまとめています：[報告](https://github.com/tatsuya0902/pix-dt295-investigation/blob/main/pix-userland/REPORT.md)／[パッチ](https://github.com/tatsuya0902/pix-dt295-investigation/blob/main/pix-userland/fix.patch)
