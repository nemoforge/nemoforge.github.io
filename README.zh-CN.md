# nemoforge

[English](README.md) | [Tiếng Việt](README.vi.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

**nemoforge** 的个人网站，用于展示 Android 和开源项目、提供联系方式，以及介绍支持开发工作的方式。

项目中配置的网站地址：**https://nemoforge.github.io/**。

## 技术栈

- React 18、TypeScript 和 Vite 5。
- 各组件使用 CSS Modules，并共享 CSS 变量。
- 使用 Lucide React 和 Simple Icons 提供图标。
- Outfit 和 Manrope 字体随应用一起打包。

这是一个静态网站，运行时不需要后端、数据库或 API 密钥。

## 功能

- **项目：** 展示名称、多语言描述、作者、版本、状态、分类、Featured 标记和项目链接。
- **联系：** 支持 Messenger、Zalo、WhatsApp、Discord、Telegram 和微信，可打开链接或复制已配置的信息。
- **支持：** 包括银行转账、MoMo、ZaloPay、Buy Me a Coffee、支付宝和微信支付。
- **语言：** 支持英语、越南语、简体中文和日语。没有保存的偏好时，自动检测浏览器语言。
- **主题：** 支持浅色、深色和跟随系统，默认跟随系统。主题与语言偏好保存在 `localStorage` 中。
- **分享：** 优先使用 Web Share API；不可用时复制规范网址（canonical URL）。
- 支持响应式布局、移动端菜单，以及遵循减少动态效果偏好的滚动显示动画。

## 本地运行

安装 Node.js 和 npm，然后在包含 `package.json` 的目录中运行：

```bash
npm ci
npm run dev
```

打开 **http://localhost:3000**。开发服务器固定使用 `3000` 端口；如果端口已被占用，请停止占用该端口的进程，或修改 `vite.config.ts`。

| 命令 | 用途 |
| --- | --- |
| `npm ci` | 根据 `package-lock.json` 安装依赖 |
| `npm run dev` | 启动 Vite 开发服务器 |
| `npm start` | 等同于 `npm run dev` |
| `npm run build` | 检查 TypeScript，并将生产构建输出到 `dist/` |
| `npm run preview` | 预览生产构建；端口可用时使用 `3000` |

在本地预览生产构建：

```bash
npm run build
npm run preview
```

当前 `package.json` 尚未定义单独的 lint 或测试脚本。

## 项目结构

```text
.
├── public/
│   ├── contact/            # 微信联系二维码
│   ├── donate/             # 收款二维码
│   ├── projects/           # 项目图标
│   ├── social/og.webp      # 分享预览图片
│   ├── logo.webp           # 页眉和页脚使用的标志
│   ├── robots.txt
│   └── site.webmanifest
├── src/
│   ├── components/         # 界面组件及其 CSS Modules
│   ├── data/               # 网站、项目、联系和支持数据
│   ├── hooks/useTheme.ts   # 浅色、深色和系统主题管理
│   ├── i18n/               # 翻译和语言管理
│   ├── styles/theme.css    # 共享颜色、字体、布局和样式
│   ├── App.tsx             # 页面组成
│   └── main.tsx            # React 入口
├── favicon.svg
├── index.html             # HTML、SEO 和分享元数据
├── package.json
├── package-lock.json
├── tsconfig.json
└── vite.config.ts
```

`dist/` 是构建生成的目录。请修改 `src/`、`public/` 或配置文件，然后重新构建。

## 内容配置

### 网站信息

编辑 [src/data/site.ts](src/data/site.ts)，可更新 GitHub 链接、规范网址、开发方向简介和分享按钮使用的内容。

品牌名称和部分文案仍直接写在组件或翻译文件中。更换品牌时，还需检查 `Header.tsx`、`Footer.tsx`、`Hero.tsx`、`src/i18n/translations.ts`、`index.html` 和 `public/site.webmanifest`。`index.html` 中的元数据不会从 `site.ts` 自动同步。

### 项目（Projects）

项目列表位于 [src/data/projects.ts](src/data/projects.ts)，当前项目为 **Clone App Profile**。

在 `projects` 数组中添加一个对象即可显示新项目：

| 字段 | 说明 |
| --- | --- |
| `id` | 项目的唯一标识 |
| `name` | 显示名称 |
| `description` | 包含 `en`、`vi`、`zh-CN`、`ja` 四个键的描述 |
| `author` | 项目作者 |
| `version` | 显示的版本，例如 `v1.0.1` |
| `status` | `stable`、`beta`、`wip` 或 `archived` |
| `category` | `module` 显示橙色标记，`apk` 显示黄色标记 |
| `featured` | 显示 Featured 标记并突出显示卡片 |
| `tags` | 技术或关键词列表 |
| `icon` | 图标路径，例如 `/projects/icon.webp` |
| `github`、`releases`、`download`、`homepage` | 对应的链接按钮，仅在配置了值时显示 |

`Project` 类型要求提供 `id`、`name`、`description` 和 `author`。如果没有图标或图片加载失败，卡片会显示项目名称的首字母。

### 工具（Tools）

Tools 位于 Projects 与 Contact 之间。在 [src/data/tools.ts](src/data/tools.ts)
中添加工具；`Tool` 使用与 `Project` 相同的字段。初始列表为空，显示“暂无工具”。
两个部分共用 `src/components/ProjectCard.tsx` 和 `Projects.module.css`，包括手机布局。
可选图标放在 `public/tools/`，使用 `/tools/<名称>.webp` 路径。
标题和空列表文案位于 `src/i18n/translations.ts` 中四种语言的 `t.tools` 下。

### 联系（Contact）

编辑 [src/data/contact.ts](src/data/contact.ts)：

| 字段 | 说明 |
| --- | --- |
| `id` | 联系渠道标识，在列表中必须唯一 |
| `label` | 卡片上显示的名称 |
| `url` | 点击 Open 按钮打开的链接 |
| `value` | 显示的姓名、用户名或联系信息 |
| `copy` | 要复制的内容；为空时使用 `value` |
| `qr` | 要打开的二维码图片路径，不显示缩略图 |

微信配置示例：

```ts
{
  id: "wechat",
  label: "WeChat",
  qr: "/contact/wechat.webp",
  copy: "/contact/wechat.webp",
}
```

- 微信的 **Open** 按钮在当前页面的对话框中显示二维码，与 Support 部分相同。
- **Copy** 会根据当前网站的源地址，将以 `/` 开头的路径转换为完整 URL。例如，在 `https://nemoforge.github.io` 上，复制结果为 `https://nemoforge.github.io/contact/wechat.webp`。
- 其他渠道优先使用 `url`，其次使用 `qr`。两者都为空时，应用会尝试根据 `value` 为 Messenger、Zalo、WhatsApp 或 Gmail 生成链接。用于生成 WhatsApp 链接的值必须是仅包含数字的电话号码。
- Discord 和 Telegram 当前使用 `url` 中直接配置的链接。
- 只有存在 `copy` 或 `value` 时才显示 Copy 按钮；无法生成链接时，Open 按钮被禁用。

如果要添加受支持 ID 之外的平台，请更新 `ContactItem` 类型，以及 `src/components/Contact.tsx` 中的 `META` 映射，设置对应图标和颜色。

### 支持（Support）

编辑 [src/data/donate.ts](src/data/donate.ts)：

| 方式 | 代码中的 ID | 行为 |
| --- | --- | --- |
| 银行转账 | `banking` | 打开包含二维码和银行转账信息的对话框 |
| MoMo | `momo` | 打开包含二维码和账户信息的对话框 |
| ZaloPay | `zalopay` | 打开二维码对话框，使用 Zalo 图标 |
| Buy Me a Coffee | `coffee` | 在新标签页中打开支持页面 |
| 支付宝 | `chinapay` | 打开支付宝二维码对话框 |
| 微信支付 | `wechatpay` | 打开微信支付二维码对话框 |

`bankName`、`accountName`、`accountNumber` 和 `transferNote` 决定二维码对话框中显示的信息行。空字段不会显示。

如果某种方式配置了 `url`，点击卡片将直接打开该 URL，而不是二维码对话框。因此，Buy Me a Coffee 使用 URL 时不需要 `coffee.webp` 图片。收款二维码缺失或加载失败时，会显示二维码尚未配置的提示。

添加新方式时，请更新 `DonationMethod` 类型、`donationMethods` 数组、`src/components/Donate.tsx` 中的图标和颜色，以及四种语言中的 `donate.hints`。

## 标志与图片

内容图片当前使用 **WebP**，网站图标使用 **SVG**。`public/` 中的文件从网站根路径提供：`public/contact/wechat.webp` 对应 `/contact/wechat.webp`，URL 中不要包含 `/public`。

| 内容 | 使用的文件 |
| --- | --- |
| 页眉和页脚标志 | `public/logo.webp` |
| 网站图标 | `favicon.svg`，由 `index.html` 引用 |
| Clone App Profile 图标 | `public/projects/icon.webp` |
| 微信联系二维码 | `public/contact/wechat.webp` |
| 银行转账二维码 | `public/donate/banking.webp` |
| MoMo 二维码 | `public/donate/momo.webp` |
| ZaloPay 二维码 | `public/donate/zalopay.webp` |
| 支付宝二维码 | `public/donate/alipay.webp` |
| 微信支付二维码 | `public/donate/wechatpay.webp` |
| 分享预览图片 | `public/social/og.webp`，声明尺寸为 1200 × 630 |

页眉标志的框为 **32 × 32px**，页脚为 **42 × 42px**，均使用 `object-fit: contain` 保持图片比例。更换标志时替换 `public/logo.webp`；调整尺寸或圆角时，修改 Header 和 Footer 的 CSS Modules 中的 `.mark` 类。

微信联系二维码与微信支付收款二维码是两张用途不同的图片。

## 语言与外观

- 编辑 [src/i18n/translations.ts](src/i18n/translations.ts)，修改四种语言的界面文案。
- 每个项目的描述保存在该项目的 `description` 字段中，而不是界面翻译字典中。
- 编辑 [src/styles/theme.css](src/styles/theme.css)，修改品牌颜色、字体、背景和共享组件样式。
- 各部分的专用样式位于对应组件旁的 `*.module.css` 文件中。
- `localStorage` 中的偏好键为 `nf-theme` 和 `nf-lang`。

## 部署

生成生产构建：

```bash
npm ci
npm run build
```

通过静态托管服务发布 **`dist/` 目录中的内容**。如果托管服务支持从源码构建，使用以下设置：

| 设置 | 值 |
| --- | --- |
| 根目录 | 包含 `package.json` 的目录 |
| 依赖安装命令 | `npm ci` |
| 构建命令 | `npm run build` |
| 输出目录 | `dist` |

Vite 当前使用 `base: '/'`，适合部署在域名根路径。源码中也有以 `/` 开头的图片路径；如果部署在子目录下，需要同时更新 base 和这些路径。

当前源码**不包含 GitHub Actions 自动部署工作流**。使用 GitHub Pages 时，需要添加发布 `dist/` 的流程；仅推送 React 源码不会生成已构建的网站。

更换域名时，请更新 `src/data/site.ts`、`index.html` 和 `package.json` 中的规范网址及元数据，以及 `public/robots.txt` 和 `public/site.webmanifest` 中的相关 URL。当前 `robots.txt` 引用了 `sitemap.xml`，但项目尚未包含该文件或生成步骤；请补充站点地图，或在不使用时删除该引用。

微信图片的 Copy 按钮自动使用当前访问的网站源地址，而 Share 按钮使用配置的 `site.canonical`。
