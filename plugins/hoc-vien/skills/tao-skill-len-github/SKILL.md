---
name: tao-skill-len-github
description: Tạo một skill mới theo yêu cầu của tôi, lưu vào tài khoản Claude và đẩy lên repo GitHub của tôi để dùng trong Claude Code. Dùng khi tôi nói tạo skill, làm skill mới, lưu skill lên GitHub.
---

# Tạo skill mới và đưa lên GitHub

Người dùng là học viên, có thể chưa rành kỹ thuật. Luôn trả lời bằng tiếng Việt, ngắn gọn, từng bước, không dùng thuật ngữ khó mà không giải thích.

Quy trình gồm 5 bước. Làm lần lượt, mỗi bước xong báo một dòng ngắn.

## Bước 1: Hỏi học viên muốn skill làm gì

Nếu học viên chưa nói đủ, hỏi bằng AskUserQuestion (hoặc hỏi trong chat nếu không có công cụ đó), tối đa 3–4 câu:
- Skill này giúp làm việc gì? Ví dụ một lần dùng thực tế.
- Khi nào thì Claude nên dùng skill này (họ sẽ nói câu gì)?
- Kết quả mong muốn là gì (văn bản, file, bài đăng, bảng...)? Có mẫu, giọng văn, quy tắc bắt buộc nào không?
- Tài khoản GitHub của họ (tên đăng nhập). Repo mặc định: `my-claude-skills`, chế độ Private.

Nếu trong cuộc trò chuyện học viên vừa làm xong một quy trình hay (ví dụ vừa viết xong bài quảng cáo theo cách họ thích), hãy đề nghị biến chính quy trình đó thành skill, dùng các bước đã làm làm nội dung.

## Bước 2: Viết skill

Đặt tên skill: chữ thường, không dấu, nối bằng gạch ngang, tối đa 64 ký tự (ví dụ `viet-bai-quang-cao-fb`). Không chứa chữ "claude" hay "anthropic".

Viết file `SKILL.md`:

```
---
name: <ten-skill>
description: <1 câu: skill làm gì + khi nào dùng, dưới 300 ký tự, không dùng dấu < >>
---

# <Tiêu đề>

<Mục tiêu của skill>

## Các bước
1. ...
2. ...

## Quy tắc / lưu ý
- ...

## Ví dụ
<một ví dụ đầu vào → đầu ra>
```

Viết hướng dẫn rõ ràng như đang chỉ cho một trợ lý mới. Cho học viên xem bản nháp (tóm tắt ngắn) và sửa theo góp ý trước khi qua bước 3.

## Bước 3: Lưu skill vào tài khoản

- **Trên Claude.ai / Cowork:** gọi công cụ `propose_skills` với skill vừa viết (kind `new`). Nhắc học viên bấm **Lưu** trên thẻ hiện ra.
- **Trên Claude Code:** ghi file vào `~/.claude/skills/<ten-skill>/SKILL.md`.

## Bước 4: Đẩy lên GitHub của học viên

Hỏi xác nhận trước khi đẩy: tên repo, Private/Public, tên skill.

Cấu trúc repo (giống nhau cho mọi học viên, để Claude Code cài được):

```
my-claude-skills/
├── .claude-plugin/marketplace.json
├── plugins/my-skills/.claude-plugin/plugin.json
├── plugins/my-skills/skills/<ten-skill>/SKILL.md
└── README.md
```

Nội dung `marketplace.json`:
```json
{
  "name": "my-claude-skills",
  "owner": { "name": "<github-username>" },
  "plugins": [
    { "name": "my-skills", "source": "./plugins/my-skills", "description": "Skill cá nhân" }
  ]
}
```

Nội dung `plugin.json`:
```json
{ "name": "my-skills", "version": "<YYYY.MM.DD>", "description": "Skill cá nhân" }
```

README.md: liệt kê các skill (tên + mô tả) và 2 lệnh cài ở Bước 5.

Trước khi đẩy, kiểm tra skill không chứa mật khẩu, token, khoá API hay thông tin cá nhân nhạy cảm; nếu có thì báo học viên và bỏ ra.

**Nếu repo đã có từ lần trước:** chỉ thêm thư mục skill mới vào `plugins/my-skills/skills/`, cập nhật bảng trong README và `version` trong plugin.json. Không xoá skill cũ.

**Cách A — có Git và `gh` đã đăng nhập** (`gh auth status` báo OK, thường là trên Claude Code):
```bash
# repo mới
cd my-claude-skills && git init -b main && git add -A && git commit -m "Thêm skill <ten-skill>"
gh repo create <user>/my-claude-skills --private --source=. --push
# repo đã có
gh repo clone <user>/my-claude-skills && cd my-claude-skills
# ... chép skill mới vào, sửa README, plugin.json ...
git add -A && git commit -m "Thêm skill <ten-skill>" && git push
```

**Cách B — dùng trình duyệt (Claude in Chrome), thường là trên Claude.ai / Cowork:**
- Tuyệt đối không tự nhập mật khẩu hay token GitHub. Nếu học viên chưa đăng nhập github.com, nhờ họ tự đăng nhập rồi báo lại.
- Repo mới: mở `https://github.com/new`, điền `my-claude-skills`, chọn Private, bấm Create repository.
- Upload từng file bằng trang `https://github.com/<user>/my-claude-skills/upload/main/<thư-mục>` và công cụ `file_upload` vào ô chọn file, rồi bấm **Commit changes**. Thư mục chưa có sẽ được tạo tự động.
- Repo mới, còn trống: upload `README.md` trước tại `https://github.com/<user>/my-claude-skills/upload` (để tạo nhánh main), sau đó mới tới các thư mục `.claude-plugin`, `plugins/my-skills/.claude-plugin`, `plugins/my-skills/skills/<ten-skill>`.
- Repo đã có: sửa README.md và plugin.json bằng cách upload bản mới đè lên đúng thư mục.
- Xong thì mở thư mục `plugins/my-skills/skills` trên GitHub, chụp màn hình để kiểm tra.
- Dự phòng: nếu không dùng được trình duyệt, nén repo thành .zip, gửi cho học viên và hướng dẫn họ tải lên GitHub bằng nút **Add file › Upload files**.

## Bước 5: Hướng dẫn dùng trong Claude Code

Gửi học viên:

```
/plugin marketplace add <user>/my-claude-skills
/plugin install my-skills@my-claude-skills
```

- Repo Private: máy phải đăng nhập GitHub trước (`gh auth login`).
- Những lần sau, khi có skill mới: `/plugin marketplace update my-claude-skills`.

## Kết thúc

Báo 2–3 dòng: tên skill vừa tạo, link repo GitHub, và 2 lệnh cài. Nhắc: "Mỗi khi muốn có skill mới, chỉ cần nói: tạo skill ..."
