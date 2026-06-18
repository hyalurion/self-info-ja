# 自己紹介（にゃんこ日本語）

Vue 3 + Vite で構築した自己紹介サイト。日本語版と漢字にふりがなを付けた仮名版を提供。

## 機能

- プライバシーポリシー同意画面
- ローディングアニメーション
- 桜吹雪のCanvasアニメーション
- 誕生日カウントダウン
- 仮名（ふりがな）表示モード切替
- 言語選択ポップアップ（日本語 / 英語 / 中国語）
- プライバシー同意書ページ
- Google Analytics 4 対応
- BGM再生

## 起動方法

```bash
npm install
npm run dev
```

## ビルド

```bash
npm run build
```

## プロジェクト構造

```
src/
├── analytics.js              # Google Analytics 4 初期化
├── main.js                   # エントリーポイント
├── App.vue                   # ルートコンポーネント
├── assets/
│   ├── anime/                # 桜アニメーション素材
│   └── styles/               # 各コンポーネント用CSS
├── components/
│   ├── sections/
│   │   ├── BirthdayCountdown.vue  # 誕生日カウントダウン
│   │   └── SectionRenderer.vue    # セクション動的レンダリング
│   ├── DocumentPage.vue      # プライバシー同意書ページ
│   ├── LanguageSelector.vue  # 言語選択
│   ├── LoadingScreen.vue     # ローディング画面
│   ├── PageFooter.vue        # フッター
│   ├── PageHeader.vue        # ヘッダー
│   ├── RichText.vue          # ルビ付きテキスト表示
│   ├── SakuraCanvas.vue      # 桜吹雪Canvas
│   ├── SplashScreen.vue      # プライバシー同意画面
│   └── UIElements.vue        # 更新ログ・BGM・仮名ボタン
├── data/
│   ├── page-content.json           # 通常版ページデータ
│   └── page-content-index-kana.json # 仮名版ページデータ
└── public/                   # 静的アセット
```

## URLパラメータ

| パラメータ | 値 | 説明 |
| --- | --- | --- |
| `?page=document` | — | プライバシー同意書ページを表示 |
| `?kana=1` | — | 仮名（ふりがな）モードを有効化 |

## デプロイ

GitHub Pagesにデプロイ。`.github/workflows/deploy.yml` により main ブランチへのpush時に自動ビルド・デプロイ。