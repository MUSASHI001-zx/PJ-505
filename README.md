# TCAS Buddy

เว็บแอปช่วยวางแผนเส้นทางเข้ามหาวิทยาลัย สร้างด้วย Python + FastAPI + Jinja2 โดยออกแบบตามภาพอ้างอิง pastel dashboard ที่มี sidebar, topbar, การ์ด และมาสคอตนักเรียน

## รันในเครื่อง

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 3000 --reload
```

## เชื่อม Supabase ภายหลัง

สร้าง Supabase project และรันไฟล์ `supabase/migrations/001_tcas_buddy.sql` ใน SQL Editor จากนั้นเปิด Authentication → Providers → Google และเพิ่ม redirect URL เป็น origin ของเว็บ ระบบต้องมี environment variables ต่อไปนี้ โดย `SUPABASE_SERVICE_ROLE_KEY` ใช้เฉพาะ backend และห้ามใส่ใน JavaScript หรือ Git:

```bash
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-publishable-key
SUPABASE_SERVICE_ROLE_KEY=your-server-key
```

หน้า Login จะใช้ Supabase JS ฝั่ง browser เฉพาะเมื่อ `/api/config` รายงานว่าค่าครบ ปัจจุบันถ้าไม่มีค่าจะแสดงโหมดพร้อมเชื่อมต่อและไม่สร้าง session ปลอม

## การตั้งค่า Google Provider

ใช้ callback ของ Supabase Auth ในรูปแบบ `https://<project-ref>.supabase.co/auth/v1/callback` ที่ Google Cloud OAuth Client และใส่ site URL/redirect URL ของเว็บใน Supabase Auth URL Configuration ตามโดเมนที่ deploy จริง

## โครงสร้างสำคัญ

`app/main.py` ให้บริการหน้าและ API, `app/repository.py` ติดต่อ Supabase REST, `templates/app.html` รวมหน้าจอ, `static/` เก็บ CSS/JS, `supabase/migrations/` เก็บ schema + RLS, `public/manus-routes.json` ประกาศ routes สำหรับ Preview/SEO

## ข้อมูลและความถูกต้อง

ข้อมูลมหาวิทยาลัย/ข่าว/กำหนดการที่อยู่ใน Preview เป็นข้อมูลตัวอย่างเพื่อสาธิต UI และมีป้ายกำกับชัดเจน สถิติแสดงขีด `—` จนกว่าจะเชื่อม Supabase ไม่ควรนำข้อมูลตัวอย่างไปใช้เป็นเกณฑ์สมัครจริง ต้องตรวจประกาศมหาวิทยาลัยหรือ myTCAS ล่าสุดเสมอ


## ระบบจัดการบัญชี (`/account`)

กดที่ชื่อผู้ใช้มุมขวาบน → **จัดการบัญชี** (หรือไอคอนเฟืองใน sidebar) จะเข้าหน้า `/account` ซึ่งมี 5 ส่วน: ข้อมูลส่วนตัว, คณะที่สนใจ (เลือกได้ไม่เกิน 10), ตั้งค่าการแจ้งเตือน (บันทึกทันทีเมื่อสลับ), ลิงก์คำเชิญ และช่วยเหลือ / ติดต่อเรา นอกจากนี้คลิกรูปโปรไฟล์เพื่อเลือกรูปจากชุดที่เตรียมไว้

**ตั้งค่าครั้งแรก:** รัน `supabase/migrations/006_account.sql` ใน Supabase SQL Editor (สร้างตาราง `user_settings`, `invite_links`, `invite_redemptions` เปิด RLS ไม่มี policy จึงเข้าถึงได้เฉพาะ backend ผ่าน service role)

**เพิ่มรูปโปรไฟล์ให้ผู้ใช้เลือก:** วางไฟล์ `.svg/.png/.webp/.jpg` ลงใน `static/assets/avatars/` (ชื่อไฟล์ใช้ได้เฉพาะ `A-Z a-z 0-9 _ -`) ระบบสแกนโฟลเดอร์เอง ไม่ต้องแก้โค้ด ชุดเริ่มต้น `a01–a12.svg` เป็นรูปตัวอย่างที่แทนที่หรือลบได้ แนะนำรูปสี่เหลี่ยมจัตุรัส ขนาดไม่น้อยกว่า 256×256

**API** (ต้องแนบ `Authorization: Bearer <supabase access token>` ยกเว้น `options`): `GET /api/account/options`, `GET|PUT /api/account`, `GET|POST /api/account/invites`, `DELETE /api/account/invites/{code}`, `POST /api/account/invites/redeem` และหน้า `/invite/{code}` ที่ส่งต่อไป `/login?invite=CODE` เพื่อรับคำเชิญหลังเข้าสู่ระบบ
