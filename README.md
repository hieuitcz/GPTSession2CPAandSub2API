# ChatGPT Session to CPA / sub2api / Cockpit / 9router / Codex / AxonHub / Codex-Manager

Công cụ web một trang chạy hoàn toàn ở phía trình duyệt, dùng để chuyển đổi session JSON đăng nhập ChatGPT Web thành JSON có thể import vào CPA, sub2api, Cockpit Tools, 9router, Codex `auth.json`, AxonHub hoặc Codex-Manager.

## Dùng online

### [**>> Mở và dùng ngay <<**](https://chatgpt.io/)

## Lưu ý

- Cách này chủ yếu phù hợp với tài khoản Plus.
- Tài khoản Free dù có `accessToken` thì thường vẫn không có quyền gọi API mô hình.
- Session từ ChatGPT Web thường không có `refresh_token`, nên khi `access_token` hết hạn sẽ không tự refresh được.

## Nguồn dữ liệu đầu vào

Bạn có thể dán trực tiếp hoặc kéo-thả JSON vào trang, gồm:

- ChatGPT Web session JSON (ví dụ chứa `user.email`, `accessToken`, `sessionToken`, `expires`, `account.id`, `account.planType`)
- 9router Codex OAuth JSON (`accessToken`, `refreshToken`, `expiresAt`, `providerSpecificData.chatgptAccountId`, `providerSpecificData.chatgptPlanType`)
- Codex native `auth.json` (`auth_mode`, `OPENAI_API_KEY`, `tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, `tokens.account_id`, `last_refresh`)
- AxonHub Codex `auth.json` (`tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, `last_refresh`)
- Codex-Manager batch import JSON (`tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, `meta.label`)

Trang cũng cố gắng đọc JWT payload từ `accessToken` để bổ sung email, account ID, user ID, gói tài khoản và thời gian hết hạn.

## Định dạng đầu ra

- `CPA`: tạo Codex CPA auth JSON, gồm `type: "codex"`, `access_token`, `session_token`, `id_token`, `email`, `account_id`, plan và thời gian hết hạn.
- `sub2api`: tạo cấu trúc `exported_at/proxies/accounts` theo định dạng tương thích sub2api; mỗi account có `expires_at` và `auto_pause_on_expired`.
- `Cockpit`: tạo token JSON dạng phẳng mà Cockpit Tools nhận diện.
- `9router`: tạo 9router Codex OAuth JSON gồm `accessToken`, `refreshToken`, `expiresAt`, `providerSpecificData`, `provider`, `authType`, `priority`, `isActive`, `createdAt`, `updatedAt`.
- `Codex`: tạo Codex native `auth.json` với `auth_mode: "chatgpt"` và `tokens.id_token/access_token/refresh_token/account_id`.
- `AxonHub`: tạo AxonHub Codex `auth.json`; nếu thiếu `refresh_token` thật sẽ dùng placeholder `__missing_refresh_token__`.
- `Codex-Manager`: tạo JSON import hàng loạt với `tokens.*` và `meta.*`.

## Sử dụng local

Mở trực tiếp:

```text
docs/index.html
```

Toàn bộ quá trình phân tích/chuyển đổi diễn ra cục bộ trong trình duyệt, không upload token và không ghi vào local storage.
