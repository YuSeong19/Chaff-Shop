# 🎮 Chaff Shop — Deploy Guide

## ไฟล์ในโปรเจค
```
chaff-shop/
├── index.html          ← แอพหลัก
├── firebase.json       ← Firebase Hosting config
├── database.rules.json ← Firebase Database rules
├── .firebaserc         ← Firebase project binding
├── vercel.json         ← Vercel config
├── .gitignore
└── README.md
```

---

## ขั้นตอนที่ 1 — เปิด Firebase Realtime Database

1. ไปที่ [Firebase Console](https://console.firebase.google.com) → **chaff-shop**
2. เมนูซ้าย → **Realtime Database** → กด **Create database**
3. เลือก Region: **asia-southeast1 (Singapore)** → กด Next
4. เลือก **Start in test mode** → กด Enable
5. **คัดลอก databaseURL** จะมีหน้าตาแบบนี้:
   ```
   https://chaff-shop-default-rtdb.asia-southeast1.firebasedatabase.app
   ```
   > ถ้า URL ไม่ตรง ให้แก้ไขใน index.html บรรทัด `databaseURL: "..."`

---

## ขั้นตอนที่ 2 — GitHub

```bash
# 1. สร้าง repo ใหม่ที่ github.com → "New repository" ชื่อ "chaff-shop"
# 2. รันคำสั่งในโฟลเดอร์ chaff-shop/

git init
git add .
git commit -m "🎮 Initial commit — Chaff Shop Topup Manager"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/chaff-shop.git
git push -u origin main
```

---

## ขั้นตอนที่ 3 — Firebase Hosting

```bash
# ติดตั้ง Firebase CLI (ครั้งแรกครั้งเดียว)
npm install -g firebase-tools

# Login
firebase login

# Deploy!
firebase deploy
```

✅ เสร็จแล้ว! URL จะขึ้นมาเช่น:
```
https://chaff-shop.web.app
https://chaff-shop.firebaseapp.com
```

---

## ขั้นตอนที่ 4 — Vercel (ทางเลือก / domain สวยกว่า)

```bash
# ติดตั้ง Vercel CLI
npm install -g vercel

# Deploy จาก GitHub (แนะนำ)
# 1. ไปที่ vercel.com → "New Project"
# 2. Import from GitHub → เลือก repo "chaff-shop"
# 3. Framework Preset: Other
# 4. กด Deploy
```

✅ URL จะเป็น:
```
https://chaff-shop.vercel.app
```

> **Auto-deploy**: ทุกครั้งที่ `git push` → Vercel จะ deploy ให้อัตโนมัติ

---

## การอัปเดตในอนาคต

```bash
# แก้ไข index.html แล้ว:
git add .
git commit -m "update: ..."
git push

# Firebase:
firebase deploy

# Vercel: auto-deploy จาก git push ได้เลย
```

---

## Database Structure (Firebase Realtime Database)

```
shop/
├── categories/    ← หมวดหมู่เกม
├── games/         ← ข้อมูลเกม
├── pkgs/          ← แพคเกจ
├── txs/           ← ประวัติการเติม
└── usage/         ← ความถี่การใช้งานต่อเกม
```

---

## ⚠️ Security Rules (สำหรับ Production)

ไฟล์ `database.rules.json` ตอนนี้เปิด read/write ทุกคน
สำหรับ Production ให้เพิ่ม Authentication ก่อน:

```json
{
  "rules": {
    "shop": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

แล้วรัน `firebase deploy --only database` เพื่ออัปเดต rules
