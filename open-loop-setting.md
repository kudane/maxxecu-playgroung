การตั้งค่า Idle Control ควรทำความเข้าใจดังนี้

---

## หลักการพื้นฐาน: Coarse vs Fine

สิ่งแรกที่ต้องจำให้ขึ้นใจคือ **Air Flow = การปรับหยาบ (Coarse)** และ **Spark Timing = การปรับละเอียด (Fine)** เพราะคนมักสับสนว่าควรปรับอะไรก่อน

ทั้งสองอย่างรวมกันสร้าง Engine Torque ที่ Idle ซึ่งนั่นคือตัวกำหนด Idle RPM

---

## ส่วนที่ 1 — Mechanical Idle (ต้องทำก่อนเสมอ)

**ทำไมต้องเริ่มจาก Mechanical ก่อน?** เพราะถ้า Air flow ไม่นิ่ง จะ tune idle ไม่ได้เลย

**วิธีทำ:**

ถอดสาย Solenoid ออก หรือไม่ต้อง define output ให้มัน จากนั้นหมุน Throttle Stop Screw ให้เครื่องอุ่นวิ่งประมาณ **Target RPM + 150-200 RPM** เช่น ถ้า target 950 RPM ให้ตั้ง Throttle ให้เครื่องวิ่งที่ 1100-1150 RPM บนเครื่องอุ่น

เหตุผลที่ต้องมี Reserve คือ Spark Timing ปรับได้เกือบทันที แต่ Air flow ปรับไม่ได้ตอน Idle ดิ่งลง ถ้ามี Reserve ไว้ เวลา Idle ดิ่ง ระบบจะ add timing เพื่อดึง RPM กลับมาได้เร็วมาก

หลังจากปรับ Throttle Stop แล้ว **ต้อง Reset TPS** ไปที่ Inputs → Sensors → Get Current Value เพื่อให้ ECU รู้ว่า Closed Throttle จริงๆ อยู่ที่ voltage เท่าไหร่

---

## ส่วนที่ 2 — Spark Timing Feedback (Idle RPM Error Table)

ไปที่ Ignition → Ignition Lock → Idle Control แล้วเปลี่ยน Ignition Angle Mode จาก Fixed เป็น **Table**

ตาราง Idle RPM Error ทำงานแบบนี้: ถ้า error = 0 แสดงว่า RPM กำลัง hit target พอดี ถ้า error เป็น **ลบ** แสดงว่า RPM เกิน target → ลด timing ลง ถ้า error เป็น **บวก** แสดงว่า RPM ต่ำกว่า target → เพิ่ม timing ขึ้น

ตาราง Target Idle RPM กำหนดตาม Coolant Temperature เช่น เครื่องเย็น 32°C อาจต้องการ 1300 RPM ส่วนเครื่องอุ่น 85°C+ ก็แค่ 950 RPM

สามารถทำ Table 3D ได้โดย right-click → Add Axis → Coolant Temp เพื่อให้ Spark Timing ที่ Idle เปลี่ยนตามอุณหภูมิด้วย

---

## ส่วนที่ 3 — PWM Solenoid Setup

ไปที่ Outputs → Output Config แล้วกำหนด Output ที่ต่อสาย Solenoid ไว้ให้เป็น **Idle PWM Solenoid** หรือ **Inverted** ถ้าสัญญาณกลับ

เรื่อง High/Low Side: Solenoid ส่วนใหญ่เป็น **Low Side** คือรับไฟ 12V แล้ว ECU ควบคุมด้านกราวด์ ใช้ GPO ทั่วไปได้ ถ้า **High Side** คือ ECU ควบคุมด้าน 12V ต้องใช้ GPO ชนิดพิเศษ

ความถี่ที่ใช้ควรตั้งที่ **200-300 Hz** ถ้าไม่รู้ให้เริ่มที่ 250 Hz

**ทดสอบ Inverted หรือไม่?** ตอนสตาร์ทเครื่องเย็น ถ้า Duty Cycle = 0 แล้ว RPM พุ่งสูงมาก แสดงว่าต้อง Invert เพราะ logic กลับกัน

---

## ส่วนที่ 4 — Open Loop Duty Cycle Tables

**Idle Duty Cycle Table** (อิงตาม Coolant Temp):
ค่า 0% = Solenoid ปิด ไม่มี Air flow เพิ่ม ค่า 100% = เปิดสุด Air flow สูงสุด บนเครื่องอุ่น **ต้องตั้งเป็น 0 เสมอ** เครื่องเย็นเริ่มจาก 35-40% แล้วค่อยๆ taper ลงตามอุณหภูมิ

**Idle Crank Duty:** Air flow ช่วงกดสตาร์ท ค่าสูงๆ เช่น 80-90% ช่วยให้ติดง่ายขึ้น เหมือนเหยียบคันเร่งเล็กน้อยตอนสตาร์ท

**Afterstart Duty Table** (อิงตาม Engine Runtime): ทำงานหลัง RPM เกิน Max Crank RPM แล้ว ให้ค่า Duty Cycle สูงช่วง 0-5 วินาทีแรก จากนั้น taper ลงเป็น 0 ภายใน 20 วินาที **คอลัมน์สุดท้ายต้องเป็น 0 เสมอ** มิฉะนั้น Air flow จะค้างไม่ลด

---

## ส่วนที่ 5 — Compensation & Idle Up

**AC Idle Up:** บวก Duty Cycle เพิ่มเมื่อ AC ทำงาน ช่วยชดเชย Torque loss จาก Compressor สามารถตั้ง RPM Offset เพิ่มด้วยได้ เช่น เพิ่ม Target เป็น 1050 แทน 950

**Clutch Idle Up:** มีประโยชน์มากกับ Twin/Triple Disc Clutch ที่มี Flywheel เบา ตอนเหยียบคลัทช์ RPM ดิ่งง่าย

**Radiator Fan Idle Up:** ช่วยเมื่อพัดลมไฟฟ้าใหญ่ทำงาน เพราะดึง Load จาก Alternator มาก

**Close Above MAP:** ตั้งให้ Solenoid ปิดเมื่อ MAP เกิน 0 psi (100 kPa) เพื่อป้องกัน Boost Leak

---

**สรุปหลักการสำคัญ:** Mechanical Idle ก่อน → Spark Feedback → เสียบ Solenoid → ตั้ง Cold Start Tables → Warm engine ต้องได้ 0% Duty Cycle เสมอ