# ⚡ MaxxECU Ignition Idle Control (แปลเฉพาะระบบ Idle)

## 🧩 Idle Angle Mode

| หัวข้อ | เนื้อหา |
|---|---|
| Idle angle mode | วิธีควบคุมรอบเดินเบาด้วย “องศาไฟจุดระเบิด” (Ignition advance) 0 |
| Disabled | ไม่ใช้ ignition ช่วยควบคุม idle |
| Idle ignition locked / by separate table | ล็อคค่าองศาไฟไว้ หรือใช้จากตารางแยก ในช่วง idle 1 |
| Idle ignition controlled by idle system | ให้ระบบ idle control ปรับไฟจุดระเบิดอัตโนมัติ เพื่อควบคุมรอบ 2 |

---

## 🎯 Ignition Source

| หัวข้อ | เนื้อหา |
|---|---|
| Ignition angle from | แหล่งที่ใช้กำหนดองศาไฟตอน idle |
| Fixed value | ใช้ค่าองศาคงที่ |
| Table | ใช้ตารางกำหนดองศาไฟตามเงื่อนไข 3 |

---

## 🔒 Idle Ignition Lock

| หัวข้อ | เนื้อหา |
|---|---|
| Idle ign angle | ค่าองศาไฟที่ใช้ เมื่อเลือก fixed value |
| หมายเหตุ | ถ้าใช้ table → ช่องขวาสุดต้องเป็น 0° เสมอ 4 |

---

## 🚫 เงื่อนไขปิดการทำงาน

| หัวข้อ | เนื้อหา |
|---|---|
| Max TPS | ปิดระบบเมื่อเหยียบคันเร่งเกินค่าที่กำหนด |
| Max RPM | ปิดระบบเมื่อรอบเกินค่าที่กำหนด (ปกติ ~2000–2500 rpm) 5 |
| Max MAP | ปิดระบบเมื่อโหลดเครื่อง (MAP) สูง |
| Max speed | ปิดระบบเมื่อรถวิ่งเกินความเร็วที่กำหนด 6 |

---

## 🎚️ ช่วงการควบคุม Ignition

| หัวข้อ | เนื้อหา |
|---|---|
| Idle control max retard angle | มุมไฟ “ต่ำสุด” ที่ระบบสามารถลดได้ (สัมพันธ์กับ 0% duty) 7 |
| Idle control max advance angle | มุมไฟ “สูงสุด” ที่ระบบสามารถเพิ่มได้ (สัมพันธ์กับ 100% duty) 8 |

---

## 🔧 การใช้ Correction อื่นร่วม

| หัวข้อ | เนื้อหา |
|---|---|
| Apply adjustments when idle ign active | กำหนดว่าจะให้ correction อื่น (เช่น CLT, IAT) มีผลกับ ignition ตอน idle หรือไม่ 9 |
| Enabled | ใช้ correction จากอุณหภูมิและอากาศ |
| Disabled | ไม่ใช้ correction ใด ๆ |

---

## ⚠️ หมายเหตุสำคัญ

| หัวข้อ | เนื้อหา |
|---|---|
| Anti-stall (เก่า) | ฟังก์ชันนี้ถูกถอดออก → ให้ใช้ ignition idle control แทน 10 |

---

## 💡 อธิบายแบบใช้งานจริง (โคตรสำคัญ)

### 🔥 หลักการ
- ECU ใช้ “ไฟจุดระเบิด” เป็นตัวปรับรอบ
- ไม่ต้องเพิ่มลม → ใช้ torque จาก ignition แทน

👉 Logic:
- ไฟอ่อน (retard) → torque ลด → รอบตก  
- ไฟแก่ (advance) → torque เพิ่ม → รอบขึ้น  

---

### 🎯 ตัวอย่างจริง

- ตั้งช่วง ignition: **4° → 36°**
- Target idle: **1000 rpm**

👉 ถ้ารอบตก:
- ECU จะ “เพิ่มไฟ” ไป 20° → 30° → รอบขึ้น

👉 ถ้ารอบสูง:
- ECU จะ “ลดไฟ” ลง → 10° → 5° → รอบลง

---

### ⚖️ ข้อดี / ข้อเสีย

#### ✅ ข้อดี
- ตอบสนอง “เร็วมาก”
- เหมาะกับ fine tuning รอบนิ่ง ๆ
- ใช้ร่วมกับ dual mode ได้ดี

#### ❌ ข้อเสีย
- ถ้า range แคบ → คุมไม่อยู่
- ถ้ากว้างเกิน → รอบแกว่งแรง
- ถ้า base ignition map มั่ว → idle เพี้ยนทันที

---

### 🧠 Insight ที่คนพลาดกันเยอะ

- Ignition = “ปรับละเอียด”
- Air (IAC / throttle) = “ปรับหยาบ”

👉 เพราะงั้น:
- Single mode ignition → ต้องตั้ง base airflow ให้ “พอดี”
- Dual mode → ignition ทำงานก่อน → air ค่อยตาม

---

## 💡 สรุปสั้น
- Ignition idle control = ใช้ไฟ “ดัน/ดึง” รอบ
- ต้องกำหนดช่วงองศาให้เหมาะ (min/max)
- มีเงื่อนไขปิดเหมือน closed loop
- ใช้ดีที่สุดเมื่อ combine กับ air control (dual mode)
