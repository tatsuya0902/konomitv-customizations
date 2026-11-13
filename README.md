# KonomiTV ローカル改造

[KonomiTV](https://github.com/tsukumijima/KonomiTV) v0.14.1 に対して自宅の録画環境で使っている独自機能のパッチと報告の置き場です。元は [PIX-DT295 調査リポジトリ](https://github.com/tatsuya0902/pix-dt295-investigation) の一部でしたが、量が増えたため分離しました。

- 機能の一覧と索引：[CUSTOM-FEATURES.md](CUSTOM-FEATURES.md)
- 改造の全記録：[LOCAL-CUSTOMIZATIONS.md](LOCAL-CUSTOMIZATIONS.md)（パッチ未収録の「視聴状態のサーバー一元化」についても、内容はここに記載しています）

## 適用方法

パッチは KonomiTV v0.14.1 の素のツリーに対して `git apply` で適用できます。全部入れる場合は以下の順序で適用してください（この順序で全適用し、本番ツリーとの差分が未収録の視聴状態一元化だけになることまで検証済み）。

```bash
for p in fix pseudo-fullscreen epgstation-integration seek-controls chase-playback \
         chase-card-click-fix episode-nav chapters excitement excitement-fix \
         digest-button-panel trash chase-live-edge recorder-extras ui-adjustments \
         quality-persistence program-watch pseudo-fullscreen-layout; do
  git apply $p.patch
done
```

- `combined-fixes.patch` / `playback-combined.patch` は初期分の一括版で、上記の一部の合本です。
- 各パッチの SHA-256 は `SHA256SUMS*` にまとめています。
- 機能ごとの詳細は [PLAYBACK.md](PLAYBACK.md)、[RECORDER-FEATURES.md](RECORDER-FEATURES.md)、[EPGSTATION-INTEGRATION.md](EPGSTATION-INTEGRATION.md)、[ADDITIONAL-FEATURES.md](ADDITIONAL-FEATURES.md) を参照してください。
