# Claude Memory Template

Template สำหรับตั้งค่า **Claude Code Memory System** ให้กับโปรเจกต์ของคุณ
ช่วยให้ Claude จำบริบทของโปรเจกต์ได้ข้ามเซสชัน

## Quick Start

### 1. Copy template ไปยังโปรเจกต์ของคุณ

```bash
# Clone template
git clone https://github.com/TanapatW/claude_memory.git

# Copy ไฟล์ที่ต้องการไปยังโปรเจกต์
cp claude_memory/CLAUDE.md /path/to/your-project/
cp -r claude_memory/.claude /path/to/your-project/
cp claude_memory/.gitignore /path/to/your-project/.gitignore  # optional
```

หรือใช้ GitHub Template:
1. กด **"Use this template"** บน GitHub
2. สร้าง repo ใหม่จาก template
3. Clone repo ใหม่ลงเครื่อง

### 2. ปรับแต่ง CLAUDE.md

เปิดไฟล์ `CLAUDE.md` แล้วกรอกข้อมูลโปรเจกต์ของคุณ:

- **Project Overview** - ชื่อ, คำอธิบาย, tech stack
- **Architecture** - โครงสร้างระบบ
- **Code Conventions** - มาตรฐานการเขียนโค้ด
- **Commands** - คำสั่งที่ใช้บ่อย
- **Important Rules** - กฎที่ Claude ต้องปฏิบัติตาม

หรือใช้คำสั่ง `/init-memory` ให้ Claude สแกนโปรเจกต์แล้วกรอกให้อัตโนมัติ

### 3. ใช้ Slash Commands

| Command | Description |
|---------|-------------|
| `/init-memory` | สแกนโปรเจกต์แล้วกรอก CLAUDE.md อัตโนมัติ |
| `/save-session` | บันทึก session notes ลง CLAUDE.md |
| `/review-code` | Review staged changes |

## File Structure

```
your-project/
├── CLAUDE.md                    # Claude's persistent memory (main file)
├── .claude/
│   ├── settings.json            # Permissions & hooks config
│   └── commands/                # Custom slash commands
│       ├── init-memory.md       # Auto-populate CLAUDE.md
│       ├── save-session.md      # Save session notes
│       └── review-code.md       # Code review command
└── .gitignore
```

## How It Works

### CLAUDE.md - Persistent Memory

`CLAUDE.md` จะถูกโหลดอัตโนมัติทุกครั้งที่เริ่ม Claude Code session
Claude จะอ่านไฟล์นี้เพื่อเข้าใจ:

- โปรเจกต์ทำอะไร
- ใช้ tech stack อะไร
- มี coding convention อะไรบ้าง
- คำสั่งที่ใช้บ่อย
- กฎสำคัญที่ต้องปฏิบัติตาม

### Memory Hierarchy (3 ระดับ)

Claude Code รองรับ memory 3 ระดับ:

| Level | File | Scope |
|-------|------|-------|
| Personal | `~/.claude/CLAUDE.md` | ใช้กับทุกโปรเจกต์ (preferences ส่วนตัว) |
| Project | `./CLAUDE.md` | ใช้กับโปรเจกต์นี้ (ทุกคนใน team เห็น) |
| Directory | `./src/CLAUDE.md` | ใช้เฉพาะ subdirectory นั้น |

### .claude/settings.json - Permissions

กำหนดสิทธิ์ที่ Claude สามารถทำได้โดยไม่ต้องถามทุกครั้ง:

```json
{
  "permissions": {
    "allow": ["Read", "Glob", "Grep", "Bash(git status)"],
    "deny": ["Bash(rm -rf *)", "Bash(git push --force*)"]
  }
}
```

### Custom Slash Commands

สร้างคำสั่งที่ใช้บ่อยเป็น markdown files ใน `.claude/commands/`:

```
.claude/commands/
├── init-memory.md      # /init-memory
├── save-session.md     # /save-session
└── review-code.md      # /review-code
```

## Customization Tips

### เพิ่ม Slash Commands ของคุณเอง

สร้างไฟล์ `.claude/commands/your-command.md` แล้วเขียน prompt ที่ต้องการ
ใช้ได้ด้วย `/your-command` ใน Claude Code

### เพิ่ม Skills

สร้าง `.claude/skills/` directory สำหรับ domain knowledge:

```
.claude/skills/
├── api-patterns.md      # API design patterns
├── database-guide.md    # Database conventions
└── testing-guide.md     # Testing standards
```

### เพิ่ม Hooks

ตั้งค่า hooks ใน `settings.json` เพื่อ automate workflows:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "cat CLAUDE.md"
          }
        ]
      }
    ]
  }
}
```

## Making This a GitHub Template

ถ้าต้องการให้ repo นี้เป็น GitHub Template:

1. ไปที่ **Settings** ของ repo
2. เลือก **"Template repository"** checkbox
3. คนอื่นจะเห็นปุ่ม **"Use this template"** บน repo page

## License

MIT - ใช้ได้อิสระ ปรับแต่งได้ตามต้องการ
