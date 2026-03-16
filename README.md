# hello_web

自分用の小さな Web ツールを保管するリポジトリです。  
基本的に単体で動く `index.html` を中心に、軽く試せる UI や実用ミニツールを置いています。

## いま入っているもの

- `index.html`
  - ルートに置いたシンプルな Hello World ページ（ガラス風デザイン）
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/

- `glassmorphism/`
  - グラスモーフィズム表現のデザインサンプル
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/glassmorphism/

- `timer/`
  - Monolith Timer（単体で動くカウントダウンタイマー）
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/timer/

- `timer2/`
  - Glass Timer（時・分・秒対応のグラスモーフィズムタイマー）
  - 夜景/夜空背景をランダム表示、開始/一時停止/再開トグル、停止で初期値（00:30:00）にリセット
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/timer2/

- `tasks/`
  - 時刻自動記録型のタスクログ UI
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/tasks/

- `md-viewer`/
  - Markdown / TXT を読み込んで見やすく表示するビューア
  - デザイン切替（スタイル×ライト/ダーク）、PDF保存（印刷）、初回全画面ドラッグ&ドロップ対応
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/md-viewer/

- `wallpaper`/
  - ワイヤーフレーム地形を描く静的な壁紙ジェネレーター
  - 氷河ブルー系のグラデーション、`R` キーで再生成、PNG（1920×1080）保存に対応
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/wallpaper/


- `wallpaper2`/
  - 3案を切替できる先進的な壁紙ジェネレーター（Neon Flux Grid / Crystal Ring Array / Data Strata Ribbons）
  - 開いた時点で描画、`R`/`1`/`2`/`3` で再生成・案切替、PNG（1920×1080）保存に対応
  - ▶ 公開ページ: https://hello-web-er0.pages.dev/wallpaper2/

## 方針

- なるべく単一 HTML で完結
- 依存を少なくして、すぐ開いて使える
- 自分で触って改善しやすい構成を優先
