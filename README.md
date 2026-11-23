# Map Documentary Shorts · Claude Skill

Skill เวอร์ชัน **1.3.0** สำหรับเขียนบทพากย์ไทย วางช็อต และทำสารคดีแผนที่แนวตั้ง **1080×1920** ด้วย Python และ FFmpeg ส่งออก MP4 พร้อมซับ Kanit ฝังในภาพและไฟล์ SRT แยก รองรับแผนที่จาก Google Earth Studio หรือ Google Earth Pro สีแบ่งพื้นที่ ลูกศรเคลื่อนไหว และป้ายชื่อที่อยู่กับตำแหน่งบนแผนที่ Google Flow ใช้ได้เมื่อเลือก

[ดาวน์โหลด map-documentary-shorts-claude.zip](./map-documentary-shorts-claude.zip?raw=true)

## ติดตั้งและเรียกใช้บน Claude

1. ดาวน์โหลดไฟล์ ZIP จากลิงก์ด้านบนตามเดิมทั้งไฟล์
2. ใน Claude เปิด **Settings → Capabilities** แล้วเปิด **Create and edit files** หากยังปิดอยู่
3. ไปที่ **Customize → Skills → + Create skill → Upload a skill** เลือกไฟล์ ZIP แล้วเปิดใช้ `map-documentary-shorts`
4. เปิดแชตใหม่แล้วพิมพ์คำสั่งด้านล่าง

> ใช้ Skill map-documentary-shorts ทำพรีวิวแนวตั้ง 6 วินาที หัวข้อ “ทำไมเรือสินค้าต้องผ่านช่องแคบมะละกา?” จากชุดตัวอย่างใน Skill ให้คาบสมุทรมลายูสีเหลืองทอง สุมาตราสีส้ม ลูกศรขาวขอบฟ้า และซับไทย Kanit ส่งออก MP4 กับ SRT ถ้ารัน FFmpeg ได้ และระบุว่าแผนที่พรีวิวมาจาก Mapzen

สำหรับงานจริง ให้แนบ PNG/JPG แนวตั้งที่ส่งออกจาก Google Earth พร้อมเครดิตที่ติดมากับภาพ และแนบเสียงพากย์ถ้ามี

> ใช้ Skill map-documentary-shorts ทำสารคดีแนวตั้ง 45 วินาทีเรื่องช่องแคบมะละกา ใช้ภาพ Google Earth ที่แนบ คงเครดิตต้นทางทุกเฟรม เขียนบทพากย์ไทย วางช็อต ทำสีแบ่งพื้นที่ ลูกศรเคลื่อนไหว และซับ Kanit แล้วเรนเดอร์ MP4 1080×1920 พร้อม SRT

[คู่มือ Skills สำหรับ Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude)

## ติดตั้งและเรียกใช้บน Claude Code

รันคำสั่งจากโฟลเดอร์โปรเจกต์ที่มี ZIP อยู่ เพื่อให้ Skill ใช้ได้กับโปรเจกต์นั้น

**Windows PowerShell**

```powershell
New-Item -ItemType Directory -Path ".claude/skills" -Force | Out-Null
Expand-Archive -LiteralPath "./map-documentary-shorts-claude.zip" -DestinationPath ".claude/skills" -Force
claude
```

**macOS, Linux หรือ Git Bash**

```bash
mkdir -p .claude/skills
unzip -o map-documentary-shorts-claude.zip -d .claude/skills
claude
```

ตรวจว่ามี `.claude/skills/map-documentary-shorts/SKILL.md` แล้วพิมพ์ในหน้าต่าง Claude Code

```text
/map-documentary-shorts ทำพรีวิวแผนที่มะละกา 6 วินาทีจากชุดตัวอย่าง ส่งออก MP4 1080×1920 และ SRT
```

[คู่มือ Skills สำหรับ Claude Code](https://code.claude.com/docs/en/skills)

## เรนเดอร์พรีวิวด้วยคำสั่งโดยตรง

ใช้ Python 3.10 ขึ้นไป, Pillow ที่รองรับ libraqm สำหรับตัวอักษรไทย และ FFmpeg ที่มีตัวเข้ารหัส libx264 อยู่ใน PATH พรีวิวที่มากับ Skill ใช้แผนที่ Mapzen และเรนเดอร์แบบไม่มีเสียงได้

```bash
python -m pip install Pillow
python -c "from PIL import features; print(features.check('raqm'))"
ffmpeg -version
python .claude/skills/map-documentary-shorts/scripts/render_map.py .claude/skills/map-documentary-shorts/assets/malacca/color-timeline.json malacca-preview.mp4
```

การตรวจ libraqm ควรแสดง `True` เมื่อใช้เสียงพากย์รวมให้เติม `--audio narration.wav` หรือใช้ `--cue-audio narration-cues/` สำหรับเสียง WAV รายวลี ดูวิธีตั้งไทม์ไลน์และรักษาเครดิตภาพที่ `references/google-earth.md` และ `references/production.md` ภายในโฟลเดอร์ Skill
