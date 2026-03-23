# ⚙️ TaskFlow Mini - ระบบหลังบ้าน (Backend Server)

ระบบ Backend RESTful API สำหรับโปรเจกต์ TaskFlow Mini พัฒนาขึ้นด้วยเทคโนโลยี Node.js, Express และฐานข้อมูล MongoDB

## 🛠️ โครงสร้างเทคโนโลยีที่ใช้งาน (Tech Stack)
- **Node.js + Express.js:** โครงสร้างพื้นฐานสำหรับการสร้างระบบ API Routing
- **MongoDB + Mongoose:** การออกแบบฐานข้อมูล Data Modeling 
- **JSON Web Token (JWT):** เสริมระบบ Authentication และความปลอดภัย
- **Cloudinary & Multer:** ระบบรองรับหน้าต่างสำหรับอัปโหลด จัดเก็บข้อมูลภาพอย่างปลอดภัย
- **Bcrypt.js:** เครื่องมือรักษาความปลอดภัยโดยการแฮชรหัสผ่านผู้ใช้งาน

## 📝 การตั้งค่าตัวแปร (Environment Variables - `.env`)
คุณต้องสร้างไฟล์ `.env` ไว้ในโฟลเดอร์ `server` โดยสามารถคัดลอกรูปแบบจาก `.env.example` ได้ตามนี้:

```env
PORT=5000                           # พอร์ตสำหรับการทำงาน (ค่าปกติคือ 5000)
MONGODB_URI=your_mongo_url_here     # ลิงก์สำหรับการเชื่อมต่อฐานข้อมูล MongoDB (จำเป็น)
JWT_SECRET=your_secret_key          # ชุดกุญแจความปลอดภัยสำหรับเข้ารหัส JWT (จำเป็น)

# ข้อมูลสำหรับตั้งค่า Cloudinary (ใช้ในการรับไฟล์รูประบบโปรไฟล์)
CLOUDINARY_CLOUD_NAME=your_name
CLOUDINARY_API_KEY=your_key
CLOUDINARY_API_SECRET=your_secret

# การตั้งค่า CORS เพื่อจัดการ URL ลิงก์ที่อนุญาต
FRONTEND_URL=http://localhost:5173  # ที่อยู่ระบบ Frontend ของคุณ
```

## 🚀 รันระบบในเครื่องของคุณ
1. เข้ามาที่โฟลเดอร์นี้: `cd server`
2. เรียกดาวน์โหลดเครื่องมือ: `npm install`
3. เริ่มต้นสภาพแวดล้อมเพื่อการพัฒนา: `npm run dev`
*(ถ้ารันด้วยค่าเริ่มต้น ระบบจะเริ่มที่พอร์ต `http://localhost:5000`)*

---

## 🌐 เอกสารข้อมูล API (API Documentations)

### ระบบผู้ใช้งาน Authentication (`/api/auth`)
| Method | Endpoint | รายละเอียด | ต้องการ Auth ไหม | ข้อมูลที่ต้องส่งไปด้วย (Body) |
|---|---|---|---|---|
| `POST` | `/register` | สมัครสมาชิกใหม่ | ไม่ต้อง | JSON: `username`, `password` |
| `POST` | `/login` | เข้าสู่ระบบและรับ Token | ไม่ต้อง | JSON: `username`, `password` |
| `GET` | `/me` | ข้อมูลโปรไฟล์ของผู้เข้าใช้ปัจจุบัน| จำเป็น | แนบ Bearer Token มาใน Cookie/Headers |
| `PUT` | `/profile` | ส่งรูปอัปเดตโปรไฟล์ (Avatar) | จำเป็น | `multipart/form-data`: `profilePic` (ประเภท File) |

### ระบบจัดการ Tasks (`/api/tasks`)
*การใช้ Routing ของระบบ Tasks ทั้งหมด ต้องมียืนยันตัวตนและมี JWT Token เท่านั้น*

| Method | Endpoint | รายละเอียดข้อมูล | ข้อมูลที่ต้องส่งไปด้วย (Body) |
|---|---|---|---|
| `GET` | `/` | โหลดรายการ Task ของผู้ใช้นี้ | - |
| `POST` | `/` | กรอกเพิ่มบันทึก Task ใหม่ | JSON: `title`, `description`, `status`, `priority`, `startDate`, `endDate` |
| `PUT` | `/:id` | อัปเดตข้อมูลแก้ไข Task ตาม id | JSON: ข้อมูลช่องใดๆ ที่ต้องการอัปเดต |
| `DELETE` | `/:id` | ลบงานทิ้งจากฐานข้อมูล | - |
