# Skill cho học viên: `tao-skill-len-github`

Mỗi khi bạn muốn có một skill mới, chỉ cần nói với Claude: **"tạo skill ..."**. Claude sẽ:

1. Hỏi bạn skill cần làm gì
2. Viết skill cho bạn
3. Lưu skill vào tài khoản Claude của bạn
4. Đẩy skill lên GitHub của bạn (repo `my-claude-skills`) để dùng trong Claude Code

## Cách cài (chọn 1 trong 2)

### Cách 1: Trên Claude.ai / ứng dụng Claude

1. Tải file [`tao-skill-len-github.zip`](./tao-skill-len-github.zip) về máy (bấm vào file › nút tải xuống).
2. Vào **Cài đặt (Settings) › Capabilities › Skills** › **Upload skill**, chọn file zip vừa tải.
3. Bật skill lên. Xong!

### Cách 2: Trên Claude Code (terminal)

```
/plugin marketplace add huyenheraspa123/claude-skill-hoc-vien
/plugin install hoc-vien@claude-skill-hoc-vien
```

## Cách dùng

Gõ cho Claude, ví dụ:

- "Tạo skill viết bài quảng cáo Facebook theo giọng văn của tôi"
- "Biến quy trình vừa làm thành skill và lưu lên GitHub"

Lần đầu Claude sẽ hỏi tên tài khoản GitHub của bạn. Hãy đăng nhập github.com trên trình duyệt trước (Claude không nhập mật khẩu thay bạn).

Sau khi skill lên GitHub, cài vào Claude Code bằng:

```
/plugin marketplace add <tên-github-của-bạn>/my-claude-skills
/plugin install my-skills@my-claude-skills
```
