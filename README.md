# nemoforge

[English](README.md) | [Tiếng Việt](README.vi.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

The personal website of **nemoforge**, showcasing Android and open-source projects, contact channels, and ways to support the work.

Configured website address: **https://nemoforge.github.io/**.

## Technology

- React 18, TypeScript, and Vite 5.
- CSS Modules for individual components, with shared CSS variables.
- Lucide React and Simple Icons for icons.
- Outfit and Manrope fonts bundled with the application.

This is a static website. No backend, database, or API keys are required to run it.

## Features

- **Projects:** names, localized descriptions, authors, versions, statuses, categories, Featured badges, and project links.
- **Contact:** Messenger, Zalo, WhatsApp, Discord, Telegram, and WeChat, with buttons to open links and copy configured information.
- **Support:** Banking, MoMo, ZaloPay, Buy Me a Coffee, Alipay, and WeChat Pay.
- **Languages:** English, Vietnamese, Simplified Chinese, and Japanese. The browser language is detected when no saved preference exists.
- **Themes:** light, dark, or system, with system as the default. Theme and language preferences are saved in `localStorage`.
- **Sharing:** uses the Web Share API when available, otherwise copies the canonical URL.
- Responsive layouts, a mobile menu, and scroll reveal effects that respect reduced-motion preferences.

## Running locally

Install Node.js and npm, then run these commands from the directory containing `package.json`:

```bash
npm ci
npm run dev
```

Open **http://localhost:3000**. The development server is configured to use port `3000` strictly. If the port is occupied, stop the process using it or change the port in `vite.config.ts`.

| Command | Purpose |
| --- | --- |
| `npm ci` | Install dependencies from `package-lock.json` |
| `npm run dev` | Start the Vite development server |
| `npm start` | Alias for `npm run dev` |
| `npm run build` | Check TypeScript and generate the production build in `dist/` |
| `npm run preview` | Preview the production build on port `3000` if available |

To preview a production build locally:

```bash
npm run build
npm run preview
```

The project does not currently define separate lint or test scripts in `package.json`.

## Project structure

```text
.
├── public/
│   ├── contact/            # WeChat contact QR image
│   ├── donate/             # Payment QR images
│   ├── projects/           # Project icons
│   ├── social/og.webp      # Social sharing preview image
│   ├── logo.webp           # Logo used in the header and footer
│   ├── robots.txt
│   └── site.webmanifest
├── src/
│   ├── components/         # UI components and their CSS Modules
│   ├── data/               # Site, project, contact, and support data
│   ├── hooks/useTheme.ts   # Light, dark, and system theme management
│   ├── i18n/               # Translations and language management
│   ├── styles/theme.css    # Shared colors, fonts, layouts, and styles
│   ├── App.tsx             # Page composition
│   └── main.tsx            # React entry point
├── favicon.svg
├── index.html             # HTML, SEO, and social metadata
├── package.json
├── package-lock.json
├── tsconfig.json
└── vite.config.ts
```

`dist/` contains generated build output. Edit `src/`, `public/`, or the configuration files, then rebuild.

## Content configuration

### Website information

Edit [src/data/site.ts](src/data/site.ts) to update the GitHub link, canonical URL, focus line, and content used by the Share button.

The brand name and some text are still written directly in components or translations. When changing the brand, also review `Header.tsx`, `Footer.tsx`, `Hero.tsx`, `src/i18n/translations.ts`, `index.html`, and `public/site.webmanifest`. Metadata in `index.html` is not automatically synchronized with `site.ts`.

### Projects

Projects are defined in [src/data/projects.ts](src/data/projects.ts). The current project is **Clone App Profile**.

Append an object to the `projects` array to display another project:

| Field | Description |
| --- | --- |
| `id` | Unique project identifier |
| `name` | Display name |
| `description` | Descriptions with all four keys: `en`, `vi`, `zh-CN`, and `ja` |
| `author` | Project author |
| `version` | Displayed version, such as `v1.0.1` |
| `status` | `stable`, `beta`, `wip`, or `archived` |
| `category` | `module` for an orange badge or `apk` for a yellow badge |
| `featured` | Displays the Featured badge and highlights the card |
| `tags` | Technologies or keywords |
| `icon` | Icon path, such as `/projects/icon.webp` |
| `github`, `releases`, `download`, `homepage` | Link buttons, displayed only when a value is configured |

The `Project` type requires `id`, `name`, `description`, and `author`. If an icon is missing or fails to load, the card displays the first letter of the project name.

### Tools

The Tools section sits between Projects and Contact. Add entries to
[src/data/tools.ts](src/data/tools.ts); `Tool` uses the same fields as `Project`.
The list starts empty and displays a localized “No tools yet” message.
Both sections share `src/components/ProjectCard.tsx` and `Projects.module.css`,
including mobile layouts. Store optional icons in `public/tools/` and reference
them as `/tools/<name>.webp`. Tools headings and empty text are in `t.tools` in
`src/i18n/translations.ts` for all four languages.

### Contact

Edit [src/data/contact.ts](src/data/contact.ts):

| Field | Description |
| --- | --- |
| `id` | Contact channel identifier, unique within the list |
| `label` | Name displayed on the card |
| `url` | Link opened by the Open button |
| `value` | Displayed name, username, or contact information |
| `copy` | Text to copy; falls back to `value` when empty |
| `qr` | QR image path to open; no thumbnail is displayed |

WeChat is configured as follows:

```ts
{
  id: "wechat",
  label: "WeChat",
  qr: "/contact/wechat.webp",
  copy: "/contact/wechat.webp",
}
```

- WeChat's **Open** button displays the QR image in an in-page dialog, like Support.
- **Copy** resolves paths starting with `/` to a full URL using the current website origin. On `https://nemoforge.github.io`, for example, the result is `https://nemoforge.github.io/contact/wechat.webp`.
- Other channels prioritize `url`, followed by `qr`. If both are empty, the app tries to construct a link from `value` for Messenger, Zalo, WhatsApp, or Gmail. A WhatsApp value used to construct a link must be a phone number containing digits only.
- Discord and Telegram currently use explicit links configured in `url`.
- The Copy button appears only when `copy` or `value` is available. The Open button is disabled when no link can be constructed.

To add a platform outside the supported IDs, update the `ContactItem` type and the `META` mapping in `src/components/Contact.tsx` with its logo and color.

### Support

Edit [src/data/donate.ts](src/data/donate.ts):

| Method | Code identifier | Behavior |
| --- | --- | --- |
| Banking | `banking` | Opens a dialog with a QR image and bank transfer details |
| MoMo | `momo` | Opens a dialog with a QR image and account details |
| ZaloPay | `zalopay` | Opens a QR dialog and uses the Zalo icon |
| Buy Me a Coffee | `coffee` | Opens the support URL in a new tab |
| Alipay | `chinapay` | Opens the Alipay QR dialog |
| WeChat Pay | `wechatpay` | Opens the WeChat Pay QR dialog |

The `bankName`, `accountName`, `accountNumber`, and `transferNote` fields determine which information rows appear in the QR dialog. Empty fields are hidden.

If a method has a `url`, its card opens that URL directly instead of displaying a QR dialog. Buy Me a Coffee therefore does not need a `coffee.webp` image while using a URL. Missing or failed payment QR images display a message indicating that the QR is not configured.

To add a method, update the `DonationMethod` type, the `donationMethods` array, the icon and color in `src/components/Donate.tsx`, and `donate.hints` in all four languages.

## Logo and images

Content images currently use **WebP**; the favicon uses **SVG**. Files in `public/` are served from the website root: `public/contact/wechat.webp` maps to `/contact/wechat.webp`. Do not include `/public` in the URL.

| Asset | File in use |
| --- | --- |
| Header and footer logo | `public/logo.webp` |
| Favicon | `favicon.svg`, referenced in `index.html` |
| Clone App Profile icon | `public/projects/icon.webp` |
| WeChat contact QR | `public/contact/wechat.webp` |
| Banking QR | `public/donate/banking.webp` |
| MoMo QR | `public/donate/momo.webp` |
| ZaloPay QR | `public/donate/zalopay.webp` |
| Alipay QR | `public/donate/alipay.webp` |
| WeChat Pay QR | `public/donate/wechatpay.webp` |
| Social preview image | `public/social/og.webp`, with declared dimensions of 1200 × 630 |

The header logo uses a **32 × 32px** frame; the footer uses **42 × 42px**. Both use `object-fit: contain` to preserve the image's aspect ratio. Replace `public/logo.webp` to change the logo, or edit the `.mark` class in the Header and Footer CSS Modules to change dimensions or corner rounding.

The WeChat contact QR and WeChat Pay payment QR are separate images with different purposes.

## Languages and appearance

- Edit [src/i18n/translations.ts](src/i18n/translations.ts) to change UI text in all four languages.
- Project descriptions belong in each project's `description` field, rather than the UI translation dictionary.
- Edit [src/styles/theme.css](src/styles/theme.css) to change brand colors, fonts, backgrounds, and shared components.
- Section-specific styles live in the `*.module.css` files beside their components.
- The preference keys in `localStorage` are `nf-theme` and `nf-lang`.

## Deployment

Create a production build:

```bash
npm ci
npm run build
```

Serve the **contents of `dist/`** using a static hosting service. If the service supports building from source, use these settings:

| Setting | Value |
| --- | --- |
| Root directory | Directory containing `package.json` |
| Dependency installation command | `npm ci` |
| Build command | `npm run build` |
| Output directory | `dist` |

Vite currently uses `base: '/'`, suitable for hosting at the root of a domain. The source also contains image paths starting with `/`. Deployment under a subdirectory requires updating both the base and these paths.

The current source tree **does not include a GitHub Actions deployment workflow**. To use GitHub Pages, add a process that publishes `dist/`; pushing the React source alone does not produce a built website.

When changing domains, update the canonical URL and metadata in `src/data/site.ts`, `index.html`, and `package.json`, along with relevant URLs in `public/robots.txt` and `public/site.webmanifest`. The current `robots.txt` references `sitemap.xml`, but the project has neither that file nor a sitemap generation step. Add a sitemap or remove the reference if it is not used.

The WeChat image Copy button automatically uses the current website origin, while the Share button uses the configured `site.canonical`.
