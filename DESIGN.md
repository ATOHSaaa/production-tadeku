# デザインガイド（蓼食う本の虫制作室サイト）

公式の Figma は無い前提で、**現行実装から読み取れるルール**をここにまとめる。変更時は `src/styles/global.css` の `@theme` とこの文書を揃えるとよい。

## プリミティブ（デザイントークン）

カラー・フォント・レイアウト幅は CSS で `:root` に近い形で宣言している（Tailwind v4 の `@theme`）。コンポーネントの `<style>` や単体 CSS からは `var(--color-brand-lime)` のように参照できる。

| トークン | 値 | 用途の目安 |
|----------|-----|------------|
| `--color-brand-lime` | `#daff0b` | アクセント、選択ハイライト、タグ、装飾線 |
| `--color-brand-orange` | `#ff6d35` | CTA・リンク強調（記事本文リンクなど） |
| `--color-brand-orange-hover` | `#ff8c5a` | オレンジのホバー |
| `--color-brand-green` | `#89b976` | 「特徴」セクション背景 |
| `--color-page-bg` | `#e6e6e6` | ページ背景 |
| `--color-text-muted` | `#666666` | メタ情報・引用 |
| `--color-text-body` | `#333333` | 補助テキスト |
| `--color-code-bg` | `#f5f5f5` | コードブロック背景 |
| `--color-code-border` | `#dddddd` | インラインコード枠 |
| `--color-border-soft` | `#e6e6e6` | 区切り線（記事ヘッダー下など） |

タイポグラフィ:

- **フォントスタック**: `Helvetica, Hiragino Sans, 游ゴシック, Yu Gothic, sans-serif`（`--font-sans`）
- **和文**: `font-feature-settings: "palt"`（プロポーショナルメトリクス）

レイアウト:

- **コンテンツ最大幅**: `--width-content` = `1080px`（トップ各セクション・一覧）
- **記事カード内本文エリア**: `--width-post` = `800px`

## コンポーネント・パターン

- **角丸カード**: 白背景 + `border: 3px solid black` + `border-radius: 16px`（実績グリッド・サービス説明・記事ヘッダーカードなど）
- **アウトラインテキスト見出し**: `-webkit-text-stroke` で縁取り風（ヘッダーロゴ・セクション英字サブタイトル）
- **帯ボタン**: 角丸 40px・太字・黒枠（「当制作室について」「詳しく見る」など）

## ブレークポイント

- **768px 以下**: 1 カラム寄せ・パディング縮小・ヒーローグリッドを縦積みなど（各コンポーネントの `@media` に記述）

## ファイル分割の考え方

| 領域 | 主な置き場所 |
|------|----------------|
| トークン + ベースリセット | `src/styles/global.css` |
| 記事（Markdown スロット含む） | `src/styles/post-article.css`（`PostLayout` のみ import） |
| セクション単位 | 各 `*.astro` の `<style>`（scoped） |

記事の更新が多い場合、**本文まわりは `post-article.css` と `PostLayout.astro` に集約**しておくと、 Markdown だけ触る運用と衝突しにくい。

## アクセシビリティ方針（最低限）

- インタラクティブ要素に **`:focus-visible`** のフォーカスリング（ヘッダーの問い合わせリンクなど）
- **`<a>` 内に `<p>` を入れない**（ボタン見た目は `<span>` + display で再現）
- マーキー等のアニメーションは **`prefers-reduced-motion: reduce` で停止**

## Tailwind

ユーティリティが必要な箇所では `@theme` で定義した色から `bg-brand-lime` のようなクラスも利用できる。現状は見た目維持のため、セクションは主にスコープ CSS で移植している。
