# nemoforge

[English](README.md) | [Tiếng Việt](README.vi.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

**nemoforge** の個人サイトです。Android やオープンソースのプロジェクト、連絡先、開発活動を支援する方法を紹介しています。

プロジェクトに設定されているサイトの URL：**https://nemoforge.github.io/**。

## 技術構成

- React 18、TypeScript、Vite 5。
- 各コンポーネントに CSS Modules を使用し、CSS 変数を共有。
- アイコンには Lucide React と Simple Icons を使用。
- Outfit と Manrope のフォントをアプリに同梱。

静的なウェブサイトのため、実行にバックエンド、データベース、API キーは必要ありません。

## 機能

- **Projects：** 名前、多言語の説明、作者、バージョン、状態、カテゴリ、Featured バッジ、プロジェクトへのリンクを表示。
- **Contact：** Messenger、Zalo、WhatsApp、Discord、Telegram、WeChat に対応。設定されたリンクを開いたり、情報をコピーしたりできます。
- **Support：** 銀行振込、MoMo、ZaloPay、Buy Me a Coffee、Alipay、WeChat Pay に対応。
- **言語：** 英語、ベトナム語、簡体字中国語、日本語に対応。保存済みの設定がない場合はブラウザーの言語を検出します。
- **テーマ：** ライト、ダーク、システム設定に対応し、初期設定はシステムに従います。テーマと言語の設定は `localStorage` に保存されます。
- **共有：** Web Share API が利用できる場合はそれを使用し、利用できない場合は正規 URL（canonical URL）をコピーします。
- レスポンシブレイアウト、モバイルメニュー、動きを減らす設定に対応したスクロール表示アニメーション。

## ローカルで実行する

Node.js と npm をインストールし、`package.json` があるディレクトリで次のコマンドを実行します。

```bash
npm ci
npm run dev
```

**http://localhost:3000** を開きます。開発サーバーのポートは `3000` に固定されています。使用中の場合は、そのポートを使っているプロセスを停止するか、`vite.config.ts` を変更してください。

| コマンド | 用途 |
| --- | --- |
| `npm ci` | `package-lock.json` に従って依存関係をインストール |
| `npm run dev` | Vite 開発サーバーを起動 |
| `npm start` | `npm run dev` と同じ動作 |
| `npm run build` | TypeScript をチェックし、本番用ビルドを `dist/` に出力 |
| `npm run preview` | 本番用ビルドをプレビュー。ポートが空いていれば `3000` を使用 |

本番用ビルドをローカルで確認するには、次を実行します。

```bash
npm run build
npm run preview
```

現在の `package.json` には、個別の lint やテスト用スクリプトは定義されていません。

## プロジェクト構成

```text
.
├── public/
│   ├── contact/            # WeChat の連絡用 QR 画像
│   ├── donate/             # 支払い用 QR 画像
│   ├── projects/           # プロジェクトのアイコン
│   ├── social/og.webp      # 共有時のプレビュー画像
│   ├── logo.webp           # ヘッダーとフッターのロゴ
│   ├── robots.txt
│   └── site.webmanifest
├── src/
│   ├── components/         # UI コンポーネントと CSS Modules
│   ├── data/               # サイト、プロジェクト、連絡先、支援のデータ
│   ├── hooks/useTheme.ts   # ライト・ダーク・システムテーマの管理
│   ├── i18n/               # 翻訳と言語の管理
│   ├── styles/theme.css    # 共通の色、フォント、レイアウト、スタイル
│   ├── App.tsx             # ページの組み立て
│   └── main.tsx            # React のエントリーポイント
├── favicon.svg
├── index.html             # HTML、SEO、共有用メタデータ
├── package.json
├── package-lock.json
├── tsconfig.json
└── vite.config.ts
```

`dist/` はビルドによって生成されるディレクトリです。`src/`、`public/`、または設定ファイルを編集してから、再ビルドしてください。

## コンテンツの設定

### サイト情報

[src/data/site.ts](src/data/site.ts) で GitHub のリンク、正規 URL、開発分野の紹介文、Share ボタンで使用する内容を更新できます。

ブランド名や一部の文言は、コンポーネントや翻訳ファイルにも直接記述されています。ブランドを変更する場合は、`Header.tsx`、`Footer.tsx`、`Hero.tsx`、`src/i18n/translations.ts`、`index.html`、`public/site.webmanifest` も確認してください。`index.html` のメタデータは `site.ts` から自動同期されません。

### Projects

プロジェクト一覧は [src/data/projects.ts](src/data/projects.ts) にあります。現在のプロジェクトは **Clone App Profile** です。

`projects` 配列にオブジェクトを追加すると、新しいプロジェクトが表示されます。

| フィールド | 説明 |
| --- | --- |
| `id` | プロジェクト固有の識別子 |
| `name` | 表示名 |
| `description` | `en`、`vi`、`zh-CN`、`ja` のすべてのキーを含む説明 |
| `author` | 作者 |
| `version` | 表示するバージョン。例：`v1.0.1` |
| `status` | `stable`、`beta`、`wip`、`archived` のいずれか |
| `category` | `module` はオレンジ、`apk` は黄色のバッジを表示 |
| `featured` | Featured バッジを表示し、カードを強調 |
| `tags` | 技術やキーワードの一覧 |
| `icon` | アイコンのパス。例：`/projects/icon.webp` |
| `github`、`releases`、`download`、`homepage` | 対応するリンクボタン。値が設定されている場合のみ表示 |

`Project` 型では `id`、`name`、`description`、`author` が必須です。アイコンがない場合や読み込みに失敗した場合は、プロジェクト名の最初の文字を表示します。

### Tools

Tools は Projects と Contact の間に表示されます。
[src/data/tools.ts](src/data/tools.ts) にツールを追加してください。
`Tool` は `Project` と同じフィールドを使用します。初期状態は空の一覧で、
「まだツールはありません。」と表示されます。両セクションは
`src/components/ProjectCard.tsx` と `Projects.module.css` を共有し、モバイル表示も共通です。
任意のアイコンは `public/tools/` に置き、`/tools/<名前>.webp` で参照します。
見出しと空の一覧の文言は `src/i18n/translations.ts` の各言語の `t.tools` にあります。

### Contact

[src/data/contact.ts](src/data/contact.ts) を編集します。

| フィールド | 説明 |
| --- | --- |
| `id` | 連絡先の識別子。一覧内で重複させないこと |
| `label` | カードに表示する名前 |
| `url` | Open ボタンで開くリンク |
| `value` | 表示する名前、ユーザー名、連絡先情報 |
| `copy` | コピーする内容。空の場合は `value` を使用 |
| `qr` | 開く QR 画像のパス。サムネイルは表示しません |

WeChat の設定例：

```ts
{
  id: "wechat",
  label: "WeChat",
  qr: "/contact/wechat.webp",
  copy: "/contact/wechat.webp",
}
```

- WeChat の **Open** ボタンは、Support と同様にページ内のダイアログで QR 画像を表示します。
- **Copy** は、`/` で始まるパスを現在のサイトのオリジンに基づく完全な URL に変換します。例えば `https://nemoforge.github.io` 上では、`https://nemoforge.github.io/contact/wechat.webp` がコピーされます。
- 他の連絡先では `url`、次に `qr` を優先します。両方が空の場合、Messenger、Zalo、WhatsApp、Gmail では `value` からリンクの生成を試みます。WhatsApp のリンク生成に使う値は、数字のみの電話番号である必要があります。
- Discord と Telegram は現在、`url` に直接設定されたリンクを使用します。
- Copy ボタンは `copy` または `value` がある場合のみ表示されます。リンクを生成できない場合、Open ボタンは無効になります。

対応済み ID 以外のサービスを追加する場合は、`ContactItem` 型と `src/components/Contact.tsx` の `META` マッピングを更新し、ロゴと色を設定してください。

### Support

[src/data/donate.ts](src/data/donate.ts) を編集します。

| 方法 | コード内の ID | 動作 |
| --- | --- | --- |
| 銀行振込 | `banking` | QR 画像と銀行振込情報のダイアログを開く |
| MoMo | `momo` | QR 画像とアカウント情報のダイアログを開く |
| ZaloPay | `zalopay` | QR ダイアログを開く。Zalo のアイコンを使用 |
| Buy Me a Coffee | `coffee` | 支援ページを新しいタブで開く |
| Alipay | `chinapay` | Alipay の QR ダイアログを開く |
| WeChat Pay | `wechatpay` | WeChat Pay の QR ダイアログを開く |

`bankName`、`accountName`、`accountNumber`、`transferNote` によって、QR ダイアログに表示される情報が決まります。空のフィールドは表示されません。

`url` が設定されている方法では、QR ダイアログの代わりにその URL を直接開きます。そのため、Buy Me a Coffee は URL を使用している間、`coffee.webp` を必要としません。支払い用 QR 画像がない場合や読み込みに失敗した場合は、QR が未設定であることを示すメッセージが表示されます。

新しい方法を追加する場合は、`DonationMethod` 型、`donationMethods` 配列、`src/components/Donate.tsx` のアイコンと色、および全 4 言語の `donate.hints` を更新してください。

## ロゴと画像

コンテンツ画像には現在 **WebP**、ファビコンには **SVG** を使用しています。`public/` 内のファイルはサイトのルートから配信されます。`public/contact/wechat.webp` の URL は `/contact/wechat.webp` です。URL に `/public` を含めないでください。

| 内容 | 使用ファイル |
| --- | --- |
| ヘッダーとフッターのロゴ | `public/logo.webp` |
| ファビコン | `favicon.svg`。`index.html` から参照 |
| Clone App Profile のアイコン | `public/projects/icon.webp` |
| WeChat の連絡用 QR | `public/contact/wechat.webp` |
| 銀行振込の QR | `public/donate/banking.webp` |
| MoMo の QR | `public/donate/momo.webp` |
| ZaloPay の QR | `public/donate/zalopay.webp` |
| Alipay の QR | `public/donate/alipay.webp` |
| WeChat Pay の QR | `public/donate/wechatpay.webp` |
| 共有プレビュー画像 | `public/social/og.webp`。宣言されているサイズは 1200 × 630 |

ロゴの枠はヘッダーが **32 × 32px**、フッターが **42 × 42px** です。どちらも `object-fit: contain` で画像の縦横比を維持します。ロゴを変更するには `public/logo.webp` を差し替え、サイズや角丸を変更するには Header と Footer の CSS Modules 内の `.mark` クラスを編集してください。

WeChat の連絡用 QR と WeChat Pay の支払い用 QR は、用途が異なる別々の画像です。

## 言語と外観

- 全 4 言語の UI 文言は [src/i18n/translations.ts](src/i18n/translations.ts) で編集します。
- プロジェクトの説明は UI の翻訳辞書ではなく、各プロジェクトの `description` フィールドにあります。
- ブランドカラー、フォント、背景、共通コンポーネントのスタイルは [src/styles/theme.css](src/styles/theme.css) で編集します。
- 各セクション固有のスタイルは、コンポーネントの隣にある `*.module.css` ファイルにあります。
- `localStorage` の設定キーは `nf-theme` と `nf-lang` です。

## デプロイ

本番用ビルドを作成します。

```bash
npm ci
npm run build
```

静的ホスティングサービスで **`dist/` の中身**を公開します。ソースコードからのビルドに対応したサービスでは、次の設定を使用します。

| 設定 | 値 |
| --- | --- |
| ルートディレクトリ | `package.json` があるディレクトリ |
| 依存関係のインストールコマンド | `npm ci` |
| ビルドコマンド | `npm run build` |
| 出力ディレクトリ | `dist` |

Vite は現在 `base: '/'` を使用しており、ドメインのルートへの配置を想定しています。ソースコードにも `/` で始まる画像パスがあります。サブディレクトリへ配置する場合は、base とこれらのパスの両方を更新してください。

現在のソースツリーには **GitHub Actions の自動デプロイワークフローがありません**。GitHub Pages を使用する場合は、`dist/` を公開する手順を追加する必要があります。React のソースコードを push するだけでは、ビルド済みのサイトは生成されません。

ドメインを変更する場合は、`src/data/site.ts`、`index.html`、`package.json` の正規 URL とメタデータ、および `public/robots.txt`、`public/site.webmanifest` の関連 URL を更新してください。現在の `robots.txt` は `sitemap.xml` を参照していますが、プロジェクトにはそのファイルや生成処理がありません。サイトマップを追加するか、使用しない場合は参照を削除してください。

WeChat 画像の Copy ボタンは現在アクセスしているサイトのオリジンを自動的に使用し、Share ボタンは設定済みの `site.canonical` を使用します。
