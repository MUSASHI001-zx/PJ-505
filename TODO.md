# TCAS Buddy — งานพัฒนา

- [ ] **ระบบ Login และ onboarding** — ผู้ใช้ล็อกอินด้วย Google ผ่าน Supabase Auth; โปรไฟล์เก็บ `id`, `name`, `email`, `role` โดย role ต้องรองรับ `student`, `parents`, `teacher`; หลังล็อกอินครั้งแรกให้เลือกบทบาท/ตอบคำถาม; บันทึกคำตอบและเวลา; หลังบ้านรวมยอดคำตอบแยกตามตัวเลือก
- [ ] **หน้า Home และสถิติ** — มี loading screen, sidebar, top search; แสดงจำนวนผู้ใช้, จำนวนเข้าชมหน้าแรก และจำนวนผู้สมัครจากข้อมูล backend; ไม่แสดงตัวเลขสมมติเป็นยอดจริงเมื่อฐานข้อมูลยังไม่พร้อม
- [ ] **หน้า News** — รวบรวมข่าว, หมวดหมู่, ค้นหา, รายละเอียด, bookmark และกดกระดิ่งติดตาม; ผู้ติดตามได้รับ in-app notification ตามหมวด; admin ส่งแจ้งเตือนเฉพาะผู้ใช้/บทบาทได้
- [ ] **หน้า Institutes** — แสดงรายชื่อมหาวิทยาลัย/สถาบันพร้อมโลโก้และชื่อ; ค้นหาและดูรายละเอียด/ลิงก์ทางการ
- [ ] **หน้า Quota Search** — ค้นหาชื่อมหาวิทยาลัย/โรงเรียน/คณะ/รอบ TCAS/เกณฑ์รับ; แสดง source และลิงก์ myTCAS; ไม่อ้าง API หรือข้อมูลรับสมัครที่ยังไม่ตรวจสอบ
- [ ] **หน้า Identity Discovery** — จำลองสถานการณ์ ตอบคำถาม และสรุปผลแนวทางคณะ/สาขา; บันทึกผลและระบุว่าเป็นเครื่องมือสำรวจตนเอง
- [ ] **หน้า Help & Community** — สร้างโพลพร้อมตัวเลือกและ tag แบบ Discord; โหวตได้หนึ่งครั้งต่อคน; AI ปรึกษาเลือกโมเดลที่ใช้งานได้; Ticket มีแชตส่วนตัวกับ admin และสายด่วน; หากไม่มี admin ให้ตอบ “ยังไม่มีแอดมินอยู่ในขณะนี้”
- [ ] **หน้า AI Tutor / Notebook ML** — สมุดโน้ต/แหล่งข้อมูล, โหมดเรียนรวมและโหมดฝึกข้อสอบ, หมวด TCAS/A-Level/TPAT/TOEIC, AI อธิบายและสร้างข้อสอบฝึกใหม่พร้อมป้ายกำกับ
- [ ] **หน้า How to / FAQ** — สอนใช้งาน Login, ข่าว/กระดิ่ง, โควตา, ตัวตน, โพล, Ticket และ AI Tutor
- [ ] **หลังบ้าน Admin** — แยกสิทธิ์ admin; dashboard สรุปผู้ใช้/role/onboarding/ผู้สมัคร/เข้าชม; จัดการข่าว/สถาบัน/เกณฑ์รับ/แจ้งเตือน/Ticket/admin presence
- [ ] **Supabase schema และ RLS** — migrations สำหรับ profiles, onboarding_answers, page_views, registrations, news, follows, bookmarks, notifications, institutions, admissions, identity_results, polls, poll_options, poll_votes, tickets, ticket_messages, admin_presence, notebooks, notebook_sources และ ai_messages; service key ต้องอยู่ backend เท่านั้น
- [ ] **การส่งมอบ** — มี `manus-routes.json`, health endpoint, README ตั้งค่า Supabase/Google, Dockerfile สำหรับ Python, ตรวจ diagnostics และเปิด Preview; checkpoint แยกจากการ publish สาธารณะ
