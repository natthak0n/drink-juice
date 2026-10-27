# ดื่มน้ำผลไม้ — วิธีสร้างไฟล์ APK ทีละขั้นตอน

## 0) ทดลองบนคอมพิวเตอร์ก่อน (ไม่บังคับ)
ดับเบิลคลิก `www/index.html` เปิดใน Chrome → กด "เริ่มเล่น" → เลื่อนเมาส์ซ้าย-ขวาแทนการเอียงเครื่อง

## 1) ติดตั้งเครื่องมือ (ทำครั้งเดียว)
1. Node.js เวอร์ชัน LTS จาก https://nodejs.org (กด Next ไปเรื่อย ๆ)
2. Android Studio จาก https://developer.android.com/studio (ติดตั้งแบบ Standard แล้วรอดาวน์โหลดส่วนประกอบให้ครบ)

## 2) เตรียมโปรเจกต์
1. แตกไฟล์ zip ไว้ เช่น `C:\drink-juice`
2. เปิด Terminal (Windows: คลิกขวาในโฟลเดอร์ > Open in Terminal)
3. พิมพ์ทีละบรรทัด:
```
npm install
npx cap add android
```

## 3) แก้ไฟล์ 2 จุด (ล็อกแนวตั้ง + ไม่ขอสิทธิ์อินเทอร์เน็ต)
เปิด `android/app/src/main/AndroidManifest.xml` ด้วย Notepad
- ในแท็ก `<activity ...` เพิ่ม `android:screenOrientation="portrait"`
- ลบบรรทัด `<uses-permission android:name="android.permission.INTERNET" />` ทิ้ง

## 4) Build เป็น APK
```
npx cap sync
npx cap open android
```
Android Studio จะเปิดขึ้น รอ Gradle sync จนแถบด้านล่างหยุดวิ่ง (ครั้งแรกอาจนาน 5-15 นาที) แล้วเลือกเมนู
**Build > Build Bundle(s) / APK(s) > Build APK(s)**
เสร็จแล้วกด "locate" ที่มุมขวาล่าง จะได้ไฟล์ `app-debug.apk`
(อยู่ที่ `android/app/build/outputs/apk/debug/`)

## 4.1) ติดตั้งลงมือถือ
1. ส่งไฟล์ APK เข้ามือถือ (สาย USB / LINE Keep / Google Drive)
2. แตะไฟล์บนมือถือ ถ้ามีข้อความเตือน ให้กดอนุญาต "ติดตั้งแอปจากแหล่งนี้"
3. กดติดตั้ง แล้วเปิดแอป "ดื่มน้ำผลไม้"

หรือเสียบสาย USB เปิด "ตัวเลือกนักพัฒนา > USB debugging" แล้วกดปุ่ม Run ▶ ใน Android Studio ได้เลย

## 5) กราฟิกและเสียง
ทุกอย่างวาดและสังเคราะห์ด้วยโค้ดใน `www/index.html` ไม่มีไฟล์ภาพ/เสียงภายนอก จึงไม่มีปัญหาลิขสิทธิ์
(ไอคอนแอปจะเป็นรูปเริ่มต้นของ Capacitor ถ้าต้องการเปลี่ยน ให้แทนไฟล์ในโฟลเดอร์ `android/app/src/main/res/mipmap-*`)

## 6) จุดที่ปรับแต่งได้ (ส่วน CFG และ JUICES ต้นไฟล์ `www/index.html`)
| ค่า | ความหมาย |
|---|---|
| START_TILT | องศาที่เริ่มดื่ม (ค่าเริ่มต้น 30) |
| DRAIN | ความเร็วน้ำลด ยิ่งมากยิ่งหมดเร็ว (0.25 ≈ 4 วินาทีเมื่อเอียงสุด) |
| WAVE | ความสูงคลื่นผิวน้ำ |
| INVERT | ถ้าผิวน้ำเอียงผิดข้าง เปลี่ยนเป็น true |
| JUICES | ชื่อและสีน้ำผลไม้ (c = สีบน, d = สีล่าง) |

แก้ไฟล์แล้วรัน `npx cap sync` แล้ว Build APK ใหม่
