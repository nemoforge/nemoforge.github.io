# Contact QR images (optional)

Place optional contact QR images here. Referenced from `src/data/contact.ts`:

- `zalo.webp`   → Zalo contact QR
- `wechat.webp` → WeChat contact QR

The Open button displays the WeChat QR in an in-page dialog. Copy still copies
the full image URL using the current website origin. Other contacts use their
configured URL first, then the QR image if no URL is set. No thumbnails are shown.
Add the actual image before using its QR path; missing files cannot be opened.
These are static local assets and are never sent to any third-party service.
