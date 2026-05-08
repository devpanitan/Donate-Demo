# 🎁 Donate Demo

ระบบแจ้งเตือนการบริจาคสำหรับ Streamer — Self-hosted, ฟรี, ไม่มีค่ารายเดือน

![TipMe](https://img.shields.io/badge/TipMe-Supported-orange?style=flat-square)
![TikTok](https://img.shields.io/badge/TikTok_LIVE-Supported-red?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## 📸 ภาพตัวอย่าง

### Dashboard
![Dashboard](screenshot/dashborad.png)

### Analytics
![Analytics](screenshot/analytics.png)

### Overlay (OBS)
![Overlay](screenshot/overlay.png)

### ทดสอบ Alert
![Alert Test](screenshot/alert-test.png)

### ทดสอบ OBS
![OBS](screenshot/test-obs.png)

### Ranks
![Ranks](screenshot/ranks.png)

### Settings
![Settings](screenshot/settings.png)

---

## ✨ ฟีเจอร์

- 🔔 **Donation Alert** — แสดงชื่อ จำนวนเงิน ข้อความ GIF และเสียงบน Stream
- 💬 **Chat Overlay** — แสดงแชท TikTok LIVE บน Stream แยก URL อิสระ
- 🔊 **TTS ภาษาไทย** — อ่านแชทและ donation ออกเสียงอัตโนมัติ
- 🏆 **Rank System** — แบ่ง tier ตามยอด แต่ละ rank มีสี GIF เสียงและ animation แยกกัน
- 📊 **Dashboard** — สถิติ กราฟ 30 วัน Top Donors และ Live Feed
- ⏱ **Queue Manager** — pause / skip / clear คิว alert ได้ real-time
- 💰 **รองรับหลายแพลตฟอร์ม** — TipMe, Tikfinity, TikTok LIVE (gifts + chat)
- 🔒 **Dashboard Password** — ป้องกันการเข้าถึง
- 💾 **MongoDB** — บันทึกทุก donation ไม่สูญหายหลัง restart

---

## ⚡ ติดตั้ง

### 1. ความต้องการของระบบ

- [Node.js](https://nodejs.org) v18+
- [MongoDB Atlas](https://cloud.mongodb.com) (free tier)
- Windows/Mac/Linux

### 2. ตั้งค่า MongoDB Atlas

1. สมัครที่ [cloud.mongodb.com](https://cloud.mongodb.com) → สร้าง Cluster (Free M0)
2. **Database Access** → สร้าง user + password
3. **Network Access** → Add IP `0.0.0.0/0`
4. **Connect** → Drivers → เลือก version **1.11 or lower** → Copy connection string

> ⚠️ ถ้า `querySrv ECONNREFUSED` → ใช้ Standard connection string (version 1.11 or lower) แทน `mongodb+srv://`

### 3. ติดตั้งโปรเจกต์

```bash
# แตกไฟล์แล้วเข้าโฟลเดอร์ (ใช้ path ที่ไม่มีภาษาไทย เช่น C:\Donate-Demo)
cd C:\Donate-Demo

# ติดตั้ง dependencies
npm install

# ติดตั้ง Chrome สำหรับ TipMe listener
npx puppeteer browsers install chrome
```

### 4. ตั้งค่า .env.local

สร้างไฟล์ `.env.local` ในโฟลเดอร์โปรเจกต์:

```env
PORT=3000
NODE_ENV=development

# MongoDB (Standard connection string — ไม่ใช้ +srv)
MONGODB_URI=mongodb://user:password@host1:27017,host2:27017,host3:27017/donate-demo?ssl=true&replicaSet=...&authSource=admin

NEXTAUTH_SECRET=donate-demo-secret-2026
NEXTAUTH_URL=http://localhost:3000

# รหัสผ่าน Dashboard
DASHBOARD_PASSWORD=admin123

TTS_ENGINE=browser
TTS_LANGUAGE=th-TH
DEFAULT_CURRENCY=THB
DEFAULT_LOCALE=th-TH
```

### 5. รัน Server

```bash
npm run dev
```

เปิด Dashboard ที่ → `http://localhost:3000/dashboard`

---

## 🎬 ตั้งค่า OBS

เพิ่ม **Browser Source** ใน OBS:

| URL | ใช้สำหรับ |
|-----|----------|
| `http://localhost:3000/overlay` | Donation Alert |
| `http://localhost:3000/overlay/chat` | Chat Overlay (TikTok) |

- ขนาด: **1920 × 1080**
- ติ๊ก ✅ **Allow transparency**

> เครื่องอื่นในวง LAN เดียวกัน → เปลี่ยน `localhost` เป็น IP เครื่อง server เช่น `192.168.1.5:3000`

---

## ⚙️ ตั้งค่าแพลตฟอร์ม

### TipMe
Dashboard → ตั้งค่า → ทั่วไป → ใส่ **TipMe Overlay URL**
(จาก tipme.in.th → Alert → URL Overlay ของคุณเอง)

### TikTok LIVE
Dashboard → ตั้งค่า → TikTok → ใส่ **username** → เปิด toggle

---

## 🐛 แก้ปัญหาที่พบบ่อย

| ปัญหา | วิธีแก้ |
|-------|---------|
| `querySrv ECONNREFUSED` | ใช้ Standard connection string หรือเปลี่ยน DNS เป็น `8.8.8.8` |
| `EINVAL path error` | ย้ายโปรเจกต์ไปไว้ที่ `C:\Donate-Demo` (ไม่มีภาษาไทย ไม่อยู่ใน OneDrive) |
| TipMe ไม่เด้ง | รัน `npx puppeteer browsers install chrome` แล้ว restart |
| Alert เด้งซ้ำ | กด Clear Queue บน Dashboard |
| TikTok chat ไม่อ่าน | เปิด "อ่านข้อความ Chat (TTS)" ใน Settings → TikTok |

---

## 🚀 Production Deploy

```bash
# VPS ด้วย PM2
npm install -g pm2
npm run build
pm2 start server.js --name donate-demo
pm2 save && pm2 startup
```

> ⚠️ **Vercel ไม่รองรับ** เพราะใช้ custom Socket.IO server
> แนะนำ: Railway.app, Render.com, DigitalOcean ($4-6/เดือน)

---

## 📄 License

MIT — ใช้งานได้ฟรี แก้ไขได้ self-host ได้
