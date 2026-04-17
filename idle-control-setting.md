# 🔧 MaxxECU Idle Control Settings (แปลเฉพาะระบบ Idle)

## 🧩 Control

| หัวข้อ | เนื้อหา |
|---|---|
| Control | โหมดการควบคุมรอบเดินเบา |
| Open loop | ใช้ตารางพื้นฐาน (base table) ในการควบคุม ต้องจูนให้เหมาะสมก่อนใช้งานโหมดอื่น |
| Closed loop (single mode) | ควบคุมรอบเดินเบาอัตโนมัติ โดยใช้ “อากาศ” หรือ “ไฟจุดระเบิด” อย่างใดอย่างหนึ่ง |
| Closed loop (dual mode) | ใช้ไฟจุดระเบิดควบคุมก่อน แล้วใช้อากาศช่วยเสริม เพื่อให้รอบนิ่งขึ้น |
| Test mode | ส่งค่า duty คงที่เพื่อทดสอบการทำงานของ idle valve หรือ stepper |

---

## 🔄 After Start

| หัวข้อ | เนื้อหา |
|---|---|
| After start duty mode | วิธีควบคุมลมหลังเครื่องติด |
| Fixed value | ใช้ค่า duty คงที่หลังสตาร์ท |
| Table | ใช้ตาราง after-start (แนะนำให้ใช้ และค่าท้ายควรเป็น 0) |

---

## 🚗 Cranking / Start

| หัวข้อ | เนื้อหา |
|---|---|
| Idle crank duty | ค่า duty คงที่ระหว่างกำลังสตาร์ทเครื่อง |
| Idle afterstart hold time | เวลาค้างค่า crank duty ก่อนเปลี่ยนไปใช้ open loop (ถ้านานจะค่อย ๆ ไล่ค่าลง) |

---

## ⚡ Load Compensation

| หัวข้อ | เนื้อหา |
|---|---|
| AC idle up | เพิ่มรอบเมื่อแอร์ทำงาน |
| Clutch idle up | เพิ่มรอบเมื่อเหยียบคลัทช์ |
| Radiator fan idle up | เพิ่ม duty ก่อนพัดลมหม้อน้ำทำงาน เพื่อลดอาการรอบตก |
| Close above MAP | ปิด idle valve (0% duty) เมื่อค่า MAP สูง |

---

## 🔁 Closed Loop Settings

| หัวข้อ | เนื้อหา |
|---|---|
| Control deadband | ช่วง RPM ที่แกว่งเล็กน้อยแล้วไม่ต้องปรับ |
| Disable over RPM | ถ้า RPM สูงเกิน target จะปิด closed loop |
| Disable over TPS | ถ้าเปิดคันเร่งเกินกำหนด จะปิด closed loop |
| Disable over speed | ถ้ารถมีความเร็วเกินกำหนด จะไม่ใช้ idle control |
| Activation delay | หน่วงเวลาก่อนเริ่มทำงาน closed loop |

---

## 📌 Activation Conditions

| หัวข้อ | เนื้อหา |
|---|---|
| Min CLT | อุณหภูมิน้ำขั้นต่ำก่อนเริ่ม closed loop |
| Min runtime | เวลาที่เครื่องต้องทำงานก่อนเปิด closed loop |

---

## 🔻 Ramp Down

| หัวข้อ | เนื้อหา |
|---|---|
| Ramp down time | เวลาลดรอบลงแบบนุ่ม หลังถอนคันเร่ง ป้องกันรอบตกดับ |

---

## 🎯 PID Control

| หัวข้อ | เนื้อหา |
|---|---|
| P gain | ตอบสนองต่อ error แบบทันที |
| I gain | สะสม error เพื่อแก้ค่าค้าง |
| D gain | ลดการแกว่งหรือ overshoot |

---

## 🎚️ Control Range

| หัวข้อ | เนื้อหา |
|---|---|
| Range mode | วิธีจำกัดช่วงการควบคุม |
| Absolute | กำหนดช่วง min/max duty แบบตายตัว (0–100%) |
| Relative | ปรับเพิ่ม/ลดจากค่า open loop |
| Min PID range | ค่าต่ำสุดที่ระบบปรับได้ |
| Max PID range | ค่าสูงสุดที่ระบบปรับได้ |

---

## 🌬️ Slow Air Follow (Dual Mode)

| หัวข้อ | เนื้อหา |
|---|---|
| Secondary air PID control | เปิด/ปิดการใช้ลมช่วยควบคุม |
| Disabled | ลมคงที่ ไม่ช่วยควบคุม |
| Enabled | ลมช่วยปรับรอบร่วมกับ ignition |
| P gain (air) | การตอบสนองของลม |
| I gain (air) | การสะสมค่าของลม |

---

## 💡

- ต้องจูน Open Loop ให้ใกล้เคียงก่อนเสมอ  
- Dual mode = ใช้ไฟควบคุมเร็ว + ลมช่วยละเอียด    
- Ramp down สำคัญต่อการกันรอบตกดับ
