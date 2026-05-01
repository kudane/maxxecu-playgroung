Closed Loop Idle Control ให้เข้าใจอย่างเป็นระบบ

---

## ความแตกต่างหลัก: Open Loop vs Closed Loop

ทำ Open Loop คือ ECU ส่งค่า Duty Cycle ตามตารางโดยไม่สนว่า RPM จะออกมาเท่าไหร่ แต่ใน Closed Loop นี้ ECU จะ "มองย้อนกลับ" เปรียบ RPM จริงกับ Target แล้วปรับตัวเองอัตโนมัติ

---

## สามโหมด: เลือกอะไรและทำไม

**โหมด 1 — Air Only** (ไม่แนะนำ)
ECU ปรับ Duty Cycle ของ Solenoid อย่างเดียวเพื่อไล่ RPM ไปหา Target ปัญหาคือ Air flow เป็น Coarse adjustment การเปลี่ยนทีละนิดทำให้ RPM กระโดดไปมามาก กลายเป็น oscillation ไม่สิ้นสุด

**โหมด 2 — Ignition Only** (สำหรับ Race engine ไม่มี Idle motor)
ECU ปรับ Spark Timing ในช่วงที่กำหนด เช่น -10° ถึง +30° โดยตรง ไม่ผ่าน Air motor เลย เหมาะกับรถที่ใช้ Throttle Stop fix air flow แล้วปล่อยให้ Spark ทำทุกอย่าง ใช้ P + D Gain, ไม่ต้องการ I Gain, และค่า D ควรสูงกว่า P ประมาณสองเท่า

**โหมด 3 — Air + Ignition** (ดีที่สุด — แนะนำเสมอถ้ามี Idle motor)
ECU ตอบสนองด้วย Spark ก่อนเป็นอันดับแรก เพราะเร็วและ fine แล้วค่อยๆ ขยับ Air flow ทีหลังในกรณีที่ Spark ไม่พอ Air จึงเป็น "backup ช้า" ส่วน Spark เป็น "primary เร็ว" ให้ P+D สำหรับ Spark และ P+I สำหรับ Air โดย Air gain ควรเล็กมาก

---

## สิ่งที่สำคัญที่สุดในทุกโหมด: Open Loop Table คือ Feed Forward

ก่อนเปิด Closed Loop ต้องตั้ง Open Loop Duty Cycle Table ให้แม่นก่อน โดยให้ RPM อยู่ภายใน ±10 RPM จาก Target ในแต่ละ Coolant Temp เหตุผลคือ Closed Loop เริ่มจากค่าในตารางนี้แล้วค่อย "ปรับเพิ่ม" ถ้าตารางผิดมาก P/I/D ต้องทำงานหนักมาก ทำให้ไม่ stable ยิ่ง Feed Forward แม่นเท่าไหร่ Gain ยิ่งต้องการน้อย ระบบยิ่ง stable

---

## รายละเอียด PID สำหรับแต่ละโหมด

สำหรับ Air mode ใช้ P + I เท่านั้น, D = 0 และค่า P ควรประมาณ 2 เท่าของ I เริ่มที่ P = 5-10 ก่อน ถ้า gain สูงเกินไปจะเห็น Duty Cycle กระโดดมาก RPM จะวิ่งขึ้นลงรุนแรง

สำหรับ Spark/Ignition mode ใช้ P + D เท่านั้น, I = 0 ค่า D ควร 2 เท่าของ P เริ่มที่ P = 5-10 และ D = 10-20 ยิ่ง D สูง Spark จะเปลี่ยนเร็วมาก เสียงเครื่องจะ "lopy" ฟังดู cam มากขึ้น ซึ่งในรถ Race ถือเป็นเรื่องปกติ

---

## Idle Duty Cycle Table — ความหมายเปลี่ยนตามโหมด (จุดที่สับสนที่สุด)

นี่คือจุดที่วิทยากรบอกว่า "confusing" มากที่สุดครับ ตาราง Idle Duty Cycle ใช้ชื่อเดียวกันแต่ความหมายต่างกันสิ้นเชิงตามโหมด ในโหมด 1 และ 3 ค่า 0-100% หมายถึง Duty Cycle ของ Air motor จริงๆ แต่ในโหมด 2 ค่า 0-100% หมายถึง "เปอร์เซ็นต์ของช่วง Spark Timing" เช่น ถ้าตั้ง Window ไว้ที่ -10° ถึง +30° แล้วใส่ค่า 50% ในตาราง นั่นหมายถึงสั่ง Spark ที่ 10° (ครึ่งกลางของช่วง) ไม่ใช่ Duty Cycle ของ Motor เลย

---

## Parameters สำคัญอื่นๆ

Deadband คือช่วง RPM ที่ ECU "ไม่ทำอะไร" แนะนำ ±50 สำหรับ Air mode และ ±25 สำหรับ Spark mode ถ้ากล้อง Cam ใหญ่มากให้เพิ่มเป็น ±100 เพื่อไม่ให้ระบบวิ่งไล่ตามความสั่นที่ธรรมชาติของ Cam

Min/Max Position ต้องกำหนดให้ตรงกับ "range จริง" ของ Solenoid เพราะถ้า Solenoid ไม่ตอบสนองตั้งแต่ 0-20% ก็ต้องตั้ง Min = 20 เพื่อไม่ให้ ECU เสียเวลา swing ในช่วงที่ไม่มีผล

Activation Delay แนะนำ 0.1-0.2 วินาทีสำหรับ Air mode แต่ใส่ 0 สำหรับ Spark mode เพราะ Spark ต้องตอบสนองทันที