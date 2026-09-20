# nemoforge

[English](README.md) | [Tiếng Việt](README.vi.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

Website cá nhân của **nemoforge**, giới thiệu các dự án Android và mã nguồn mở, tập hợp kênh liên hệ và phương thức ủng hộ.

Tên miền được cấu hình trong dự án: **https://nemoforge.github.io/**.

## Công nghệ

- React 18, TypeScript và Vite 5.
- CSS Modules cho từng component, kết hợp biến CSS dùng chung.
- Lucide React và Simple Icons cho biểu tượng.
- Font Outfit và Manrope được đóng gói cùng ứng dụng.

Website tĩnh, không cần backend, cơ sở dữ liệu hay API key để chạy.

## Tính năng

- **Projects:** tên, mô tả đa ngôn ngữ, tác giả, phiên bản, trạng thái, danh mục, nhãn Featured và các liên kết dự án.
- **Contact:** Messenger, Zalo, WhatsApp, Discord, Telegram và WeChat; có nút mở và sao chép thông tin đã cấu hình.
- **Support:** Banking, MoMo, ZaloPay, Buy Me a Coffee, Alipay và WeChat Pay.
- **Ngôn ngữ:** tiếng Anh, tiếng Việt, tiếng Trung giản thể và tiếng Nhật. Tự nhận diện ngôn ngữ trình duyệt khi chưa có lựa chọn được lưu.
- **Giao diện:** sáng, tối hoặc theo hệ thống; mặc định theo hệ thống. Lựa chọn giao diện và ngôn ngữ được lưu trong `localStorage`.
- **Chia sẻ:** dùng Web Share API khi khả dụng, nếu không thì sao chép URL canonical.
- Bố cục thích ứng với màn hình nhỏ, menu di động và hiệu ứng xuất hiện khi cuộn có hỗ trợ giảm chuyển động.

## Chạy trên máy

Cần cài Node.js và npm. Chạy các lệnh tại thư mục chứa `package.json`:

```bash
npm ci
npm run dev
```

Mở **http://localhost:3000**. Dev server được cấu hình cố định ở cổng `3000`; nếu cổng đang bận, cần dừng tiến trình đang dùng cổng hoặc sửa `vite.config.ts`.

| Lệnh | Chức năng |
| --- | --- |
| `npm ci` | Cài dependency theo `package-lock.json` |
| `npm run dev` | Chạy Vite ở chế độ phát triển |
| `npm start` | Tương đương `npm run dev` |
| `npm run build` | Kiểm tra TypeScript rồi tạo bản build trong `dist/` |
| `npm run preview` | Xem bản build tại cổng `3000` nếu cổng còn trống |

Để xem bản production trên máy:

```bash
npm run build
npm run preview
```

Dự án hiện chưa khai báo lệnh lint hoặc test riêng trong `package.json`.

## Cấu trúc dự án

```text
.
├── public/
│   ├── contact/            # QR liên hệ WeChat
│   ├── donate/             # QR của các phương thức ủng hộ
│   ├── projects/           # Icon dự án
│   ├── social/og.webp      # Ảnh xem trước khi chia sẻ
│   ├── logo.webp           # Logo được dùng ở header và footer
│   ├── robots.txt
│   └── site.webmanifest
├── src/
│   ├── components/         # Các phần giao diện và CSS Modules tương ứng
│   ├── data/               # Dữ liệu website, dự án, liên hệ và ủng hộ
│   ├── hooks/useTheme.ts   # Quản lý giao diện sáng/tối/hệ thống
│   ├── i18n/               # Bản dịch và bộ quản lý ngôn ngữ
│   ├── styles/theme.css    # Màu sắc, font, layout và style dùng chung
│   ├── App.tsx             # Ghép các phần của trang
│   └── main.tsx            # Điểm khởi chạy React
├── favicon.svg
├── index.html             # HTML, SEO và metadata chia sẻ
├── package.json
├── package-lock.json
├── tsconfig.json
└── vite.config.ts
```

`dist/` là kết quả build; chỉnh nội dung trong `src/`, `public/` hoặc các file cấu hình rồi build lại.

## Cấu hình nội dung

### Thông tin website

Sửa [src/data/site.ts](src/data/site.ts) để cập nhật GitHub, URL canonical, dòng giới thiệu lĩnh vực và nội dung dùng cho nút chia sẻ.

Tên thương hiệu và một số nội dung vẫn được viết trực tiếp trong component hoặc bản dịch. Khi đổi thương hiệu, kiểm tra thêm `Header.tsx`, `Footer.tsx`, `Hero.tsx`, `src/i18n/translations.ts`, `index.html` và `public/site.webmanifest`. Metadata trong `index.html` không tự đồng bộ từ `site.ts`.

### Projects

Danh sách nằm trong [src/data/projects.ts](src/data/projects.ts). Dự án hiện có là **Clone App Profile**.

Thêm một object vào mảng `projects` để hiển thị dự án mới:

| Trường | Ý nghĩa |
| --- | --- |
| `id` | Mã duy nhất của dự án |
| `name` | Tên hiển thị |
| `description` | Mô tả với đủ khóa `en`, `vi`, `zh-CN`, `ja` |
| `author` | Tác giả |
| `version` | Phiên bản hiển thị, ví dụ `v1.0.1` |
| `status` | `stable`, `beta`, `wip` hoặc `archived` |
| `category` | `module` màu cam hoặc `apk` màu vàng |
| `featured` | Hiển thị nhãn Featured và làm nổi bật thẻ |
| `tags` | Danh sách công nghệ hoặc từ khóa |
| `icon` | Đường dẫn icon, ví dụ `/projects/icon.webp` |
| `github`, `releases`, `download`, `homepage` | Các nút liên kết; chỉ xuất hiện khi có giá trị |

`id`, `name`, `description` và `author` là các trường bắt buộc theo kiểu `Project`. Khi không có icon hoặc ảnh tải lỗi, thẻ dùng chữ cái đầu của tên dự án.

### Tools

Mục Tools nằm giữa Projects và Contact. Thêm công cụ vào
[src/data/tools.ts](src/data/tools.ts); kiểu `Tool` dùng cùng các trường với `Project`.
Danh sách ban đầu để trống và hiển thị “Chưa có công cụ”. Hai mục dùng chung
`src/components/ProjectCard.tsx` và `Projects.module.css`, bao gồm bố cục điện thoại.
Đặt icon trong `public/tools/` và dùng đường dẫn `/tools/<tên>.webp`.
Tiêu đề và thông báo trống nằm trong `t.tools` của cả bốn ngôn ngữ ở
`src/i18n/translations.ts`.

### Contact

Sửa [src/data/contact.ts](src/data/contact.ts):

| Trường | Ý nghĩa |
| --- | --- |
| `id` | Mã kênh liên hệ, duy nhất trong danh sách |
| `label` | Tên hiển thị trên thẻ |
| `url` | Liên kết mở khi nhấn Open |
| `value` | Tên, username hoặc thông tin hiển thị |
| `copy` | Nội dung sao chép; nếu trống thì dùng `value` |
| `qr` | Đường dẫn ảnh QR để mở, không hiển thị ảnh thu nhỏ |

WeChat được cấu hình như sau:

```ts
{
  id: "wechat",
  label: "WeChat",
  qr: "/contact/wechat.webp",
  copy: "/contact/wechat.webp",
}
```

- **Open** của WeChat hiển thị ảnh QR trong hộp thoại ngay trên trang, giống mục Support.
- **Copy** chuyển đường dẫn bắt đầu bằng `/` thành URL đầy đủ theo địa chỉ website đang mở. Ví dụ, trên `https://nemoforge.github.io`, kết quả là `https://nemoforge.github.io/contact/wechat.webp`.
- Các kênh khác ưu tiên `url`, sau đó đến `qr`. Nếu cả hai trống, ứng dụng thử tạo link từ `value` cho Messenger, Zalo, WhatsApp hoặc Gmail. Với WhatsApp, giá trị dùng tạo link phải là số điện thoại gồm các chữ số.
- Discord và Telegram đang dùng link cấu hình trực tiếp trong `url`.
- Nút Copy chỉ xuất hiện khi có `copy` hoặc `value`; nút Open bị vô hiệu hóa khi không tạo được liên kết.

Khi thêm một nền tảng mới ngoài các ID được hỗ trợ, cập nhật kiểu `ContactItem` và bảng `META` trong `src/components/Contact.tsx` để thêm logo và màu.

### Support

Sửa [src/data/donate.ts](src/data/donate.ts):

| Phương thức | ID trong mã nguồn | Hành vi |
| --- | --- | --- |
| Banking | `banking` | Mở hộp QR và thông tin chuyển khoản |
| MoMo | `momo` | Mở hộp QR và thông tin tài khoản |
| ZaloPay | `zalopay` | Mở hộp QR, dùng biểu tượng Zalo |
| Buy Me a Coffee | `coffee` | Mở URL ủng hộ trong tab mới |
| Alipay | `chinapay` | Mở hộp QR Alipay |
| WeChat Pay | `wechatpay` | Mở hộp QR WeChat Pay |

Các trường `bankName`, `accountName`, `accountNumber`, `transferNote` quyết định những dòng thông tin xuất hiện trong hộp QR. Trường để trống sẽ không hiển thị.

Nếu một phương thức có `url`, thẻ mở trực tiếp URL đó thay vì mở hộp QR. Vì vậy Buy Me a Coffee không cần ảnh `coffee.webp` khi đang dùng URL. Khi QR trong hộp ủng hộ bị thiếu hoặc tải lỗi, ứng dụng hiển thị thông báo chưa cấu hình QR.

Để thêm phương thức mới, cập nhật kiểu `DonationMethod`, mảng `donationMethods`, icon/màu trong `src/components/Donate.tsx` và `donate.hints` của cả bốn ngôn ngữ.

## Logo và ảnh

Ảnh nội dung hiện dùng **WebP**; favicon dùng **SVG**. Các file trong `public/` được phục vụ từ gốc website: `public/contact/wechat.webp` tương ứng với `/contact/wechat.webp`, không thêm `/public` vào URL.

| Nội dung | File đang sử dụng |
| --- | --- |
| Logo header và footer | `public/logo.webp` |
| Favicon | `favicon.svg`, được tham chiếu trong `index.html` |
| Icon Clone App Profile | `public/projects/icon.webp` |
| QR liên hệ WeChat | `public/contact/wechat.webp` |
| QR ngân hàng | `public/donate/banking.webp` |
| QR MoMo | `public/donate/momo.webp` |
| QR ZaloPay | `public/donate/zalopay.webp` |
| QR Alipay | `public/donate/alipay.webp` |
| QR WeChat Pay | `public/donate/wechatpay.webp` |
| Ảnh chia sẻ | `public/social/og.webp` — kích thước khai báo 1200 × 630 |

Logo header giữ khung **32 × 32px**, footer **42 × 42px**, dùng `object-fit: contain` để giữ tỷ lệ ảnh. Thay `public/logo.webp` để đổi logo; sửa class `.mark` trong CSS Modules của Header và Footer nếu cần đổi kích thước hoặc bo góc.

QR liên hệ WeChat và QR thanh toán WeChat Pay là hai ảnh riêng, dùng cho hai mục đích khác nhau.

## Ngôn ngữ và giao diện

- Sửa [src/i18n/translations.ts](src/i18n/translations.ts) để đổi nội dung giao diện trong bốn ngôn ngữ.
- Mô tả từng dự án nằm trong `description` của dự án đó, không nằm trong từ điển giao diện.
- Sửa [src/styles/theme.css](src/styles/theme.css) để đổi màu chủ đạo, font, nền và các thành phần dùng chung.
- Style riêng của từng phần nằm trong file `*.module.css` cạnh component.
- Hai khóa lưu tùy chọn trong `localStorage` là `nf-theme` và `nf-lang`.

## Triển khai

Tạo bản build:

```bash
npm ci
npm run build
```

Phục vụ **nội dung thư mục `dist/`** bằng dịch vụ hosting tĩnh. Nếu dịch vụ hỗ trợ build từ mã nguồn, dùng:

| Thiết lập | Giá trị |
| --- | --- |
| Thư mục gốc | Thư mục chứa `package.json` |
| Lệnh cài dependency | `npm ci` |
| Lệnh build | `npm run build` |
| Thư mục đầu ra | `dist` |

Cấu hình Vite hiện dùng `base: '/'`, phù hợp khi website nằm ở gốc tên miền. Mã nguồn cũng có đường dẫn ảnh bắt đầu bằng `/`; nếu triển khai dưới thư mục con, cần cập nhật cả base và các đường dẫn này.

Bản mã nguồn hiện tại **chưa có workflow GitHub Actions triển khai tự động**. Nếu dùng GitHub Pages, cần bổ sung quy trình xuất bản `dist/`; chỉ push mã nguồn React chưa tạo thành bản website đã build.

Khi đổi tên miền, cập nhật URL canonical và metadata trong `src/data/site.ts`, `index.html`, `package.json`, cùng các URL liên quan trong `public/robots.txt` và `public/site.webmanifest`. `robots.txt` hiện tham chiếu `sitemap.xml`, nhưng dự án chưa có file hoặc bước tạo sitemap; cần bổ sung sitemap hoặc bỏ dòng tham chiếu nếu không sử dụng.

Nút Copy ảnh WeChat tự dùng tên miền đang truy cập, còn nút Share dùng `site.canonical` đã cấu hình.
