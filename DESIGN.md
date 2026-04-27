# DESIGN — shogi

## 2026-04-27: 単一 HTML ファイル構成

**判断**: ビルド・依存・サーバ不要、`index.html` 1 ファイルに JS / CSS / 棋譜データを全部埋め込む。

**Why**:
- ローカルで開く・GitHub Pages に置く、どちらも追加設定なしで即動く
- 棋譜ビューアは UI 1 画面・状態が少ない・外部 API 不要 — フレームワークを使う理由がない
- 「触らない」で凍結する前提なので、ファイル分割はメンテコスト純増

**代替案**:
- React + Vite — 将来の拡張余地は広がるが、ビルド・node_modules・GitHub Pages 設定の手間が現状価値より大きい
- 棋譜を別 JSON にする — `KIFU` 定数 1 個なので分離メリットなし

**超越条件**: 任意棋譜の貼り付け・KIF/CSA インポート・複数局保存などで状態が増えたら、その時点で React か Lit 等への移行を再検討。

## 2026-04-27: 棋譜文字列 → 盤面の解決方式

**判断**: 棋譜は人間可読の `▲`/`△`+座標+駒種+修飾子（`成` `打` `(封)` 等）で記述、from（移動元）は `reachableSquares()` で逆算。

**Why**:
- 入力が公開棋譜（新聞・中継）の表記そのままなので、`KIFU` 定数を手で書ける・読める
- 同型駒が複数 from 候補を持つケースは「動けない方を除外」で一意に決まる将棋ルール上の特性を利用

**代替案**:
- KIF/CSA フォーマット（from が明示）— 機械可読だが手で書きにくい。インポート機能を作るときに使う

## 2026-04-27: 独立リポ化（`mhlw-ec-pharmacy-finder` から分離）

**判断**: 棋譜ビューアは `mhlw-ec-pharmacy-finder`（緊急避妊薬 EC 検索）と無関係なので、`odakin/shogi` として独立。

**Why**: ドメインが完全に直交。pharmacy 側に置き続けると `Auto: update data` の cron commit に混ざって履歴が読みづらくなる、`docs/` ディレクトリの用途を曖昧にする、`gh-pages` 設定の干渉リスクなど。

**経緯**: 前セッション（online Claude）で誤って pharmacy リポのブランチ `claude/shogi-game-viewer-3vQBu` に commit `c81b4bc` として置いてしまい、削除コミット `c43ebb1` で消した。本リポはその commit から `docs/shogi/index.html` を `git show` で復元して作成。pharmacy 側のブランチは origin から削除（中身ゼロ・net diff zero）。
