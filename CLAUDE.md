# shogi — 将棋棋譜ビューア

単一 HTML ファイルの将棋棋譜ビューア。ブラウザで `index.html` を開くだけで動作、外部依存ゼロ・ビルド不要・GitHub Pages 即公開可。

## リポジトリ情報

- パス: `<base>/shogi/`
- ブランチ: `main`
- リモート: `odakin/shogi` (public, GitHub)

## 構造

```
shogi/
├── index.html           # 棋譜ビューア本体（680 行・自己完結）
├── CLAUDE.md            # このファイル
├── SESSION.md           # 現在の作業状態
├── DESIGN.md            # 設計判断
├── README.md / README.ja.md
├── .gitignore
└── .claude/
    └── public-repo.marker
```

## 動作

- ブラウザで `index.html` を開くと第 84 期名人戦第 2 局（▲藤井聡太 − △糸谷哲郎、89 手）が読み込まれる
- 9×9 盤・駒台・棋譜リスト・自動再生・キーボード操作（`←` `→` `Space`）対応
- 後手駒は CSS `transform: rotate(180deg)`、成駒は赤字、最終手マスは黄色ハイライト

## データ構造（`index.html` 内 JS）

- `KIFU` 定数: 棋譜文字列をそのまま埋め込み。`▲`/`△` で先後、`同` で前手と同じマス、`成`/`馬`/`龍`/`と` で成駒、`打` で持駒打、`(封)` で封じ手マーカー
- `MOVES` テーブル: 駒種ごとの移動可能ベクトル
- `reachableSquares()`: 到達可能マスから from を逆算（棋譜に from 情報がないため）

ローカル動作確認:

```sh
python3 -m http.server -d ~/Claude/shogi 8000
# http://localhost:8000/
```

## 触らない方針

`index.html` のパーサ・盤面リゾルバは Node で全 89 手の合法性を検証済。回帰させないため**触らない**。拡張する場合は別ファイル分離を検討。

## 将来の拡張候補

- 任意の棋譜貼り付けフォーム
- KIF / CSA インポート
- 棋譜 URL パラメータ化（`?kifu=...`）

## How to Resume

1. `SESSION.md` を読む
2. 残タスクに従って作業継続
3. 変更後は commit + push（`<base>/CONVENTIONS.md` §3）

## 安全規則（公開リポ）

このリポは public。`<base>/CONVENTIONS.md` §5 に従い、実名・メールアドレス・所属機関名・他ユーザー名・非公開リポ名を絶対にコミットしない。`.claude/public-repo.marker` 経由で leak-guard hook が走る。
