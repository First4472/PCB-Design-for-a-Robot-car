บทที่ 3
หลักการ แนวคิด และการออกแบบโครงงาน

ในบทนี้ทางคณะผู้จัดทำจะอธิบายภาพรวมของระบบ หลักการ แนวคิด ในการทำหุ่นยนต์
3.1 ภาพรวมของระบบ

![1](https://github.com/user-attachments/assets/28953ac6-cbbf-4007-9293-6ebd543c50d1)


รูปที่ 3.1  ภาพรวมโครงงาน
โครงงานนี้มีวัตถุประสงค์เพื่อออกแบบแผ่นวงจรพิมพ์ (PCB) สำหรับรถหุ่นยนต์ โดยใช้ซอฟต์แวร์ Easy EDA ในการออกแบบและจำลองวงจร PCB ให้มีความแม่นยำและมีประสิทธิภาพสูงสุด PCBจากภาพรวมโครงงานในรูปที่ 3.1 นั้นสามารถอธิบายได้ดังนี้โดยโครงงานนี้จะมีส่วนของฮาร์ดแวร์ดังนี้ ESP32-CAM 1ตัว, ESP32-Wroom-32 1ตัว, FT232RL Mini USB 1ตัวL298N 1ตัว, DC Motor 4ตัวและ IR Line Tracking Module 2 แผ่นพีซีบีสำหรับรถหุ่นยนต์ Li-po Battery 2200mAh 1 ตัวส่วนในด้านของซอฟต์แวร์มีดังนี้ Arduino, Easy Edaและ Flowchart การทำงานโดยรวมของโครงงานจะเป็นดังรูปที่ 3.2

![image](https://github.com/user-attachments/assets/01d01774-c94d-4fc1-b730-83dbda44d0d3)


รูปที่ 3.2 Flowchart การทำงานโดยรวมของโครงงาน

3.1.1 การออกแบบระบบการควบคุมรถโรบอท
โดยหลักการทำงานของรถโรบอท จะแบ่งการทำงานออกเป็น 2 ส่วน คือส่วนที่ใช้ ESP32-CAM ซึ่งเป็นการใช้งานแบบควบคุมด้วยมือ (Manual Control) และส่วนที่ใช้ ESP32 WROOM เป็นระบบอัตโนมัติ (Auto) ซึ่ง 2 บอร์ดนี้แบ่งหน้าที่การทำงานกันตามจุดประสงค์ ในส่วนแรกนี้เราจะพูดถึงการทำงานโดยใช้ บอร์ด  ESP32-CAM โดยฟังก์ชันการทำงานของโหมดนี้จะมี 2 การทำงานคือ การบังคับด้วยมือ (Manual Control) และ ฟังก์ชันการแทร็กตามเส้นสีดำ (Line Follower) ซึ่งอธิบายสิ่งต่างๆที่เกี่ยวกับโหมดนี้ได้ดังนี้
	ส่วน ESP32-CAM อธิบายการทำงานของ Infrared Sensor เบื้องต้น
ฟังก์ชันการทำงานของการแทร็กตามเส้นคือเมื่อเราจะทำการเขียนโค้ดให้เซนเซอร์อินฟราเรด
ทำการตรวจจับวัตถุที่มีแสงสีดำ ถ้าตรวจจับได้ จะทำการได้รับค่าอินพุตเป็น 0 แต่ในทางกลับกัน ขณะที่เซนเซอร์ไม่สามารถทำการตรวจจับวัตถุที่มีสีดำได้ จะได้รับค่าเป็น 1
3.1.2 การบังคับด้วยมือ (Manual Control) 
เป็นการควบคุมทิศทางการเคลื่อนที่ของรถด้วยมือเราโดยตรงผ่าน DC Motor ในการบังคับแบบ Manual จะมีการบังคับผ่านหน้า Websocket โดยจะมีปุ่มButtonให้ผู้บังคับสามารถเลือกทิศทางการเคลื่อนที่ได้อย่างอิสระทั้งเดินหน้าเลี้ยวซ้ายขวา ถอยหลัง โดยผู้ใช้จำเป็นที่ต้องเชื่อมต่อกับ Wi-Fi ที่ตัวบอร์ด ESP32-CAM เป็นตัวปล่อยสัญญาณ แล้วเข้าบราว์เชอร์แล้วค้นหา 192.168.4.1 เพื่อทำการบังคับรถ

![image](https://github.com/user-attachments/assets/ea377d5f-ddce-4fb3-96bc-9639f40201ed)

  
รูปที่ 3.3 เกี่ยวกับการเชื่อมต่อเพื่อบังคับด้วยมือ

3.1.3 การต่อวงจรของโหมดESP32-CAM

![image](https://github.com/user-attachments/assets/a3c66ac5-f91b-4aa8-9bdc-447a2a07c9ec)

 
รูปที่ 3.4 การต่อวงจรในส่วนของการบังคับโรบอทด้วย ESP32-CAM

จากรูปที่ 3.4 อธิบายการต่อวงจร ในวงจรนี้ใช้ ESP32-CAM เป็นตัวควบคุมหลักในการบังคับมอเตอร์ทั้ง 4 ตัวผ่านโมดูล L298N ซึ่งเป็นวงจร H-Bridge ที่ช่วยในการควบคุมทิศทางการหมุนของมอเตอร์แต่ละตัว การเชื่อมต่อไฟฟ้าเริ่มจากแบตเตอรี่ขนาด 11.1V (2200 mAh) ที่ใช้เป็นแหล่งจ่ายพลังงานหลักให้กับระบบ ขั้วบวกของแบตเตอรี่ต่อเข้ากับขั้ว 12V ของโมดูล L298N ส่วนขั้วลบต่อเข้ากับ GND ของ L298N เพื่อสร้างวงจรไฟฟ้าที่สมบูรณ์และจ่ายพลังงานให้มอเตอร์ทั้ง 4ตัว สำหรับการควบคุมมอเตอร์นั้น โมดูล L298N จะเชื่อมต่อกับ ESP32-CAM โดยตรง พอร์ต IN1, IN2, IN3, และ IN4 ของ L298N ถูกกำหนดให้เชื่อมต่อกับขา GPIO ของ ESP32-CAM เพื่อกำหนดทิศทางการหมุนของมอเตอร์แต่ละตัว สายไฟที่ต่อออกจากพอร์ตเหล่านี้ไปยังขามอเตอร์จะกำหนดทิศทางการหมุนของมอเตอร์ ด้านหน้าและหลังของรถ โดยขั้ว Out1 และ Out2 จะเชื่อมต่อกับมอเตอร์ซ้ายหน้า ขณะที่ Out3 และ Out4 เชื่อมต่อกับมอเตอร์ขวาหน้า นอกจากนี้ขั้ว Out1 และ Out2 อีกหนึ่งชุดจะเชื่อมต่อกับมอเตอร์ซ้ายหลัง และ Out3 และ Out4 อีกชุดจะเชื่อมต่อกับมอเตอร์ขวาหลัง ESP32-CAM ทำหน้าที่เป็นตัวประมวลผลหลัก โดยรับคำสั่งควบคุมทิศทางผ่านโมดูล L298N การส่งสัญญาณจาก GPIO ของ ESP32-CAM ไปยังโมดูล L298N จะกำหนดทิศทางและการหมุนของมอเตอร์ ในวงจรนี้ยังมีการติดตั้งสวิตช์เพิ่มเติมระหว่างขั้วบวกของแบตเตอรี่กับโมดูล L298N เพื่อให้สามารถเปิดและปิดวงจรไฟฟ้าหลักได้ตามต้องการ

![image](https://github.com/user-attachments/assets/d89ca2ca-974f-46be-915d-f604f2ae0955)


รูปที่ 3.5 Flowchart การทำงานของ ESP32-CAM

3.2 การทำงานของโค้ดที่ใช่ควบคุมรถโรบอท
โปรแกรมสำหรับควบคุมรถที่ใช้ ESP32-CAM ผ่าน WebSocket และสามารถควบคุมการเคลื่อนที่ได้ผ่าน Web Interface รวมถึงสามารถใช้เซ็นเซอร์ตรวจจับเส้นเพื่อนำทางรถได้โดยอัตโนมัติ (Line Following Mode)

3.2.1 ไลบรารีที่ใช้

![image](https://github.com/user-attachments/assets/f60fcee6-a0a2-4434-b951-513d2af628d8)

 
รูปที่ 3.6 ไลบรารีที่ใช้

- esp_camera.h → ใช้ควบคุมกล้องของ ESP32-CAM
- WiFi.h → ใช้จัดการการเชื่อมต่อ WiFi
- AsyncTCP.h และ ESPAsyncWebServer.h → ใช้สร้างเซิร์ฟเวอร์แบบ Asynchronous
- iostream และ sstream → ใช้จัดการข้อความที่ส่งผ่าน WebSocket

3.2.2 การกำหนดค่าเริ่มต้น

![image](https://github.com/user-attachments/assets/a52c377d-ce41-40ef-89b0-107779959d80)

 
รูปที่ 3.7 การกำหนดค่าเริ่มต้น

- ตัวแปร mode ใช้กำหนดว่า ESP32-CAM จะควบคุมรถด้วยตนเองหรือจะใช้เซ็นเซอร์นำทางอัตโนมัติ

3.2.3 ฟังก์ชันควบคุมมอเตอร์

![image](https://github.com/user-attachments/assets/20ecfdd4-1bec-4cc8-8c37-630d35ba6aa6)

 
รูปที่ 3.8 ฟังก์ชันควบคุมมอเตอร์

-ควบคุมการหมุนของมอเตอร์โดยกำหนดขา IN1 และ IN2 ของมอเตอร์แต่ละตัว

![image](https://github.com/user-attachments/assets/851bf733-e127-4cf6-bfba-1735ec27f222)

 
รูปที่ 3.9 ควบคุมการหมุนของมอเตอร์

- move Ca r(int inputValue) ใช้ควบคุมทิศทางของรถโดยเรียก rotateMotor() สำหรับมอเตอร์ซ้ายและขวา

3.2.4 การจัดการ WebSocket สำหรับควบคุมรถ

![image](https://github.com/user-attachments/assets/b693bb52-aaa0-4955-a82a-267d4d5af860)

 
รูปที่ 3.10 การจัดการ WebSocket สำหรับควบคุมรถ

- เมื่อได้รับข้อมูลผ่าน WebSocket จะทำการประมวลผลค่าที่ได้รับ
- ตรวจสอบว่าข้อความที่ได้รับมีคำสั่งอะไร เช่น "MoveCar" หรือ "Speed"
- หากเป็น "MoveCar" จะเรียก moveCar(valueInt); เพื่อควบคุมรถ
- หากเป็น "Speed" จะกำหนดค่าความเร็วของ PWM
- หากเป็น "Mode" จะเปลี่ยนโหมดของรถ (Manual/Auto)

3.2.5 การตั้งค่า Web Server

![image](https://github.com/user-attachments/assets/d26c00d7-20d3-4868-9ce6-204e330ce167)

 
รูปที่ 3.11 การตั้งค่า Web Server

- handleRoot() → ส่งหน้า HTML ไปยังผู้ใช้
- handleNotFound() → ส่งข้อความแจ้งเมื่อหาไฟล์ไม่พบ

![image](https://github.com/user-attachments/assets/cbfb545a-0da8-4a6a-8ace-f18deabe4784)

 
รูปที่ 3.12 WiFi Access Point

- เปิด WiFi Access Point ให้ ESP32 สร้างเครือข่ายของตัวเอง

3.2.6 การตั้งค่ากล้อง ESP32-CAM

![image](https://github.com/user-attachments/assets/82cefad6-9ddf-461a-b309-f2f1fc29a1d1)

 
รูปที่ 3.13 การตั้งค่ากล้อง ESP32-CAM

- กำหนดค่าเริ่มต้นให้กับ ESP32-CAM
- ตั้งค่าคุณภาพของรูปภาพ (jpeg_quality = 10)
- เปิดใช้งานเซ็นเซอร์และปรับให้ภาพถูกต้อง (s->set_vflip(s, 1); และ s->set_hmirror(s, 1);)

3.2.7 ฟังก์ชันการส่งภาพจากกล้อง

![image](https://github.com/user-attachments/assets/20caf4da-2ad7-494b-b53b-c93dc1a4c9c4)

 
รูปที่ 3.14 ฟังก์ชันการส่งภาพจากกล้อง

ถ่ายรูปจากกล้อง และส่งผ่าน WebSocket ไปยัง Web Interface
3.2.8 การควบคุมเซ็นเซอร์นำทางอัตโนมัติ

![image](https://github.com/user-attachments/assets/4e9b43fc-20d8-4bb4-a397-20b2e00f5b99)

 
รูปที่ 3.15 การควบคุมเซ็นเซอร์นำทางอัตโนมัติ

หากโหมดอัตโนมัติถูกเปิดใช้งาน (mode == 1)
- ถ้าทั้งสองเซ็นเซอร์ตรวจพบเส้น ให้รถเคลื่อนที่ไปข้างหน้า
- ถ้าเซ็นเซอร์ซ้ายเจอเส้น ให้หมุนไปทางซ้าย
- ถ้าเซ็นเซอร์ขวาเจอเส้น ให้หมุนไปทางขวา
- ถ้าไม่พบเส้น ให้หยุดรถ

3.3 ส่วนของ Easy Eda
EasyEDA เป็นเครื่องมือสำหรับงานด้าน Electronic Design Automation (EDA) ที่ใช้ออกแบบวงจรพิมพ์ PCB  (สามารถจำลองวงจรได้ด้วย แต่งานที่โดดเด่นกว่าคือออกแบบลายวงจรพิมพ์) EasyEDA ทำงานแบบออนไลน์ นั่นก็หมายความว่าขณะที่ออกแบบนั้นจำเป็นต้องเชื่อมต่อกับอินเตอร์เน็ต ผู้ใช้งานสามารถใช้งานโปรแกรมผ่านทางโปรแกรมเปิดดูเวปเช่น google chrome, Microsoft Edge หรือโปรแกรมอื่น ๆ ที่ใช้เปิดเวป และยังสามารถดาวน์โหลดตัวโปรแกรมใช้งานมาติดตั้งบนเครื่องเพื่อทำงานได้เช่นกันการใช้งาน โปรแกรม EasyEDA ดำเนินการดังนี้

3.3.1 เข้าเว็ปไซต์ https://easyeda.com จะปรากฏดังรูปการใช้งาน โปรแกรม Easy EDA ดำเนินการดังนี้

![image](https://github.com/user-attachments/assets/8ec03958-0826-474b-aefe-8bc484d5d84e)

 
รูปที่ 3.16 เข้าเว็ปไซต์ https://easyeda.com

3.3.2 สำหรับผู้ที่ยังไม่ได้สมัครใช้งาน ให้ดำเนินการสมัครโดยคลิกที่ Register

![image](https://github.com/user-attachments/assets/35faa5f2-bef4-4f91-8e7e-314a4100881d)

 
รูปที่ 3.17 สมัครใช้งาน

3.3.3 เริ่มเข้าใช้งานโดยคลิกที่ Easy EDA Designer

![image](https://github.com/user-attachments/assets/b5d82a09-b4ec-4f39-8d36-7bbde4aaa701)

 
รูปที่ 3.18 เริ่มเข้าใช้งานโดยคลิกที่ Easy EDA Designer


3.3.4 คลิกที่ Std Edition (สำหรับการใช้งานฟรี) 

![image](https://github.com/user-attachments/assets/963d7ed3-9f44-4ae7-940a-1a584472bde5)

 
รูปที่ 3.19 คลิกที่ Std Edition (สำหรับการใช้งานฟรี)



3.3.5 หน้าตาโปรแกรมที่ใช้งานผ่านโปรแกรมเปิดเวปไซต์

![image](https://github.com/user-attachments/assets/9c291285-cb0f-48f8-a302-95e638479b95)

รูปที่ 3.20 หน้าตาโปรแกรมที่ใช้งานผ่านโปรแกรมเปิดเว็ปไซต์

3.3.6 กรณีที่ผู้ใช้งานต้องการโปรแกรมทำงานที่ไม่ใช่โปรแกรมเปิดดูเว็ป สามารถดาวน์โหลดมาติดตั้งได้โดยคลิกที่ Desktop Client

![image](https://github.com/user-attachments/assets/cf005384-33f9-4dc8-a9f8-ddb663a9e690)

 
รูปที่ 3.21 แบบไม่ใช่โปรแกรมเปิดดูเว็ป

3.3.7 เปิดโปรแกรมมาจะมีหน้าตาเดียวกันกับตอนเปิดจากโปรแกรมเปิดเว็ป
   -เริ่มการใช้งานโดยการสร้างโปรเจคงานใหม่ คลิกที่ New Project หรือคลิกที่เมนู File แล้วเลือก New Project

![image](https://github.com/user-attachments/assets/b222fa69-6b75-45fc-8061-c7e14674abf1)

 
รูปที่ 3.22 เปิดโปรแกรมมาจะมีหน้าตาเดียวกันกับตอนเปิดจากโปรแกรมเปิดเว็ป

3.3.8 ตั้งชื่อโปรเจคงานในช่อง Title:

![image](https://github.com/user-attachments/assets/0e086423-72f9-453f-baf9-349f20e75004)

 
รูปที่ 3.23 ตั้งชื่อโปรเจคงานในช่อง Title:


3.3.9 ตัวโปรแกรมจะสร้างไฟล์เอกสารสำหรับวาดวงจรดังรูป

![image](https://github.com/user-attachments/assets/d736e84c-59cc-4541-a1ae-3a88a284c70d)

 
รูปที่ 3.24 ตัวโปรแกรมจะสร้างไฟล์เอกสารสำหรับวาดวงจร

3.3.10 อุปกรณ์สำหรับวาดวงจรโปรแกรมจัดให้มีอุปกรณ์พื้นฐานที่มักใช้บ่อยสามารถคลิกได้ที่เมนูด้านข้างชื่อ Commonly Library

 ![image](https://github.com/user-attachments/assets/034c916d-6c4f-4cc4-898c-da691b31fb53)

รูปที่ 3.25 อุปกรณ์สำหรับวาดวงจรโปรแกรม

3.3.11 แต่ละตัวโปรแกรมมีให้เลือกใช้หลากหลายตัวถัง ซึ่งสามารถคลิกเลือกใช้งานให้ตรงกับความต้องการได้

 ![image](https://github.com/user-attachments/assets/fde0cf06-97cd-4361-b223-6aa7e169acd3)

รูปที่ 3.26 อุปกรณ์สำหรับวาดวงจรโปรแกรม

3.3.12 กรณีที่หาใน Commonly Library แล้วไม่มีสามารถคลิกที่ปุ่ม Library เพื่อค้นหาเพิ่มเติมได้

 ![image](https://github.com/user-attachments/assets/ff5cd9fc-9865-494f-9734-36bd6b21604f)

รูปที่ 3.27 Commonly Library

3.3.13 อุปกรณ์ที่ใช้ทั้งหมดในการทำ EasyEDA 

 ![image](https://github.com/user-attachments/assets/6e93facf-0ddb-485b-af8b-eb1d8867df03)

รูปที่ 3.28 อุปกรณ์ที่ใช้ทั้งหมดในการทำ EasyEDA

3.3.14 Capacitors 200uF 35V
https://www.lcsc.com/product-detail/Aluminum-Electrolytic-Capacitors---SMD_DMBJ-RVT1V221M0810-220UF-35V_C970697.html 

 ![image](https://github.com/user-attachments/assets/70483f75-2ccd-4fea-8f6a-24f427616b41)

รูปที่ 3.29 Capacitors 200uF 35V

3.3.15 Capacitors 0.1uF
https://www.es.co.th/detail.asp?prod=009200155

![image](https://github.com/user-attachments/assets/eb14bee8-8da1-4217-bb8a-55502e9f4dcd)
 
รูปที่ 3.30 Capacitors 0.1uF

3.3.16 Capacitors 220uF
https://www.agebkk.com/product/220uf-16v-85c-aluminum-electrolytic-capacitors-elna-6x11mm/11000940281003801

![image](https://github.com/user-attachments/assets/174db8dc-328c-4a2a-9755-fd43d31f8576)
  
รูปที่ 3.31 Capacitors 220uF

3.3.17 Capacitors 10uF
https://agencyelectronics.com/th/products/759449-capacitors-10uf-16v-85c-nichicon-5x11mm

![image](https://github.com/user-attachments/assets/bc0e3bb4-c2cd-4dc7-9d50-fffa85099661)
 
รูปที่ 3.32 ตัวเก็บประจุ 100nF

3.3.18 Connector ของตัว XT60PWE
    https://easyeda.com/component/08b3e4729ccc4fad9d7bfc4320d8b41 
    (1) คลิก Device Standardlzation
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป
    (3) หารายการที่มีคำว่า XT60PWE หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C98732
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

 ![image](https://github.com/user-attachments/assets/c2453e4c-faf0-4e32-98fa-6c5763cf64a8)

รูปที่ 3.33 XT60PWE

3.3.19 Diode ของตัว 1N4007
https://www.lcsc.com/product-detail/Diodes---General-Purpose_-DIOTEC--1N4007_C212822.html
    (1) คลิก Device Standardlzation
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป
    (3) หารายการที่มีคำว่า 1N4007 หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C106903
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

 ![image](https://github.com/user-attachments/assets/ea53e5a3-2515-4046-a4b2-5b8a805764d0)

รูปที่ 3.34 1N4007

3.3.20 Sensor ของตัว H2,H3 LED ของตัว H4
   https://www.lcsc.com/product-detail/Pin-Header-Female-Header_Shenzhen-Cankemeng-22025403P00CKMT_C146690.html
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป
    (3)หารายการที่มีคำว่า B-2200S03P-A120 หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C146690
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

 ![image](https://github.com/user-attachments/assets/f6047cff-b31d-439b-8039-db2ce38b561e)

รูปที่ 3.35 Sensor ของตัว H2,H3 LED ของตัว H4

3.3.21 jumper ของตัว J1คือC66690
    https://www.lcsc.com/product-detail/Pin-Headers_BOOMELE-Boom-Precision-Elec-   C66690_C66690.html 
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป
    (3)หารายการที่มีคำว่า 2.54-2*2P หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C66690
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

![image](https://github.com/user-attachments/assets/7339fcf7-92c1-47fd-9c8d-49a349efc11a)
 
รูปที่ 3.36 jumper j1

3.3.22 jumper ของตัว J2,J3 คือตัวC124375
https://easyeda.com/components/HDR-M-2-54-1X2_9a0e93703c8a408fb1f0573508575b2c
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป
    (3)หารายการที่มีคำว่า B-2100S02P-A110 หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C124375
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

![image](https://github.com/user-attachments/assets/c11ca73b-b5a9-46bc-9e6e-3dc5f4a19bae)
 
รูปที่ 3.37 jumper j1
3.3.23 ปุ่มกด Korean Hroparts Elec K2-1102DP-E4SW-04
  https://www.lcsc.com/product-detail/Tactile-Switches_Korean-Hroparts-Elec-K2-1102DP-E4SW-04_C136684.html     
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป 
    (3)หารายการที่มีคำว่า K2-1102DP-E4SW-04 หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C136684
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)
    
 ![image](https://github.com/user-attachments/assets/fd00c524-9d0b-49ad-a450-7c2c12d20c10)

รูปที่ 3.38 ปุ่มกด
3.3.24 CONN-TH_2P-P5.00
https://easyeda.com/components/PKT-TH-2P-P5-00-V-F-DB301R-5-2P_ec5dbaf312484f008173d8484a265be0

 ![image](https://github.com/user-attachments/assets/dfffdc0f-1349-46e8-ad31-e182b934bea1)

รูปที่ 3.39 CONN-TH_2P-P5.00
3.3.25 ESP32-cam
https://www.lcsc.com/product-detail/WiFi-Modules_Ai-Thinker-ESP32-CAM_C277946.html 
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป 
    (3)หารายการที่มีคำว่า ESP32-CAM หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ ESP32-CAM
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

 ![image](https://github.com/user-attachments/assets/301e2fb8-9aa9-4959-9c62-cfb46d92b982)

รูปที่ 3.40 ESP32-cam

3.3.26 L298N
https://jlcpcb.com/partdetail/MSKSEMI-L298N_MS/C19632282  
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป 
    (3)หารายการที่มีคำว่า L298N(MS) หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C19632282
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

 ![image](https://github.com/user-attachments/assets/9adbef85-59b9-4b92-9439-eeac372e8509)

รูปที่ 3.41 L298N

3.3.27 FTDI MODULE RED
https://himalayansolution.com/product/ftdi-module-red

![image](https://github.com/user-attachments/assets/6f802be6-0081-49c4-b229-06cd622d8cf0)
 
รูปที่ 3.42 FTDI MODULE RED

3.3.28 Voltage Regulator 7805(TO-220) 
https://www.allnewstep.com/product/357/7805-voltage-regulator-ic-5v-1-5a-to-220  
    (1) คลิก Device Standardlzation 
    (2) ทำการค้นหาโดยใช้ข้อความค้นหาดังรูป 
    (3)หารายการที่มีคำว่า L7805CV-DG หากไม่เจอให้ดูเบอร์ใกล้เคียงที่มีรายละเอียดของตัวถังตามต้องการคือ C3795
    (4) ให้ดูว่าอุปกรณ์ตัวนี้มีพร้อมทั้งสัญลักษณ์ (Symbol) และตัวถัง (Foot Print)

![image](https://github.com/user-attachments/assets/d25c94aa-e632-4f6b-b84f-f853bcc45bac)

รูปที่ 3.43 7805(TO-220)
3.3.29 สวิทช์ KCD1-101A

![image](https://github.com/user-attachments/assets/b23ab1eb-d4d0-4743-817d-54bedcdbfdad)
 
รูปที่ 3.44 สวิทช์ KCD1-101A

3.3.30 เมื่อวางครบจะเป็นดังรูป

![image](https://github.com/user-attachments/assets/9240eeb8-c874-4142-8297-4e90bb3a4a4d)
 
รูปที่ 3.45 วางอุปกรณ์

 ![image](https://github.com/user-attachments/assets/2ebcb663-2a52-424d-ade0-82d837fba36e)

รูปที่ 3.46 วางอุปกรณ์


3.3.31 คลิกที่ปุ่ม Convert Schematic to PCB ดังรูป เพื่อตรวจสอบความถูกต้องของตัวถัง

![image](https://github.com/user-attachments/assets/e8f310db-6d5a-4ec3-a680-b73081193709)
 
รูปที่ 3.47 ตรวจสอบความถูกต้อง

3.3.32 คลิก Apply

![image](https://github.com/user-attachments/assets/b491fb27-0b6a-4364-bace-24db2ec2dd32)
 
รูปที่ 3.48 คลิก Apply

3.3.33 ตรวจสอบความถูกต้องของระยะห่างของขาอุปกรณ์แต่ละตัวและขนาดของตัวถัง หากต้องการดูว่าอุปกรณ์แต่ละตัวมีโมเดล 3 มิติหรือไม่ให้คลิกที่ 3D

![image](https://github.com/user-attachments/assets/06eafb8f-496d-4ff9-9bf7-e54d38df0d9d)
 
รูปที่ 3.49 ตรวจสอบความถูกต้องของระยะห่างของขาอุปกรณ์แต่ละตัวและขนาดของตัวถัง

3.3.34 ผลที่ได้ (กรณีอุปกรณ์ตัวใดไม่มีโมเดล 3 มิติสามารถเลือกอุปกรณ์ตัวใหม่ หรือค้นหาโมเดล 3 มิติตัวอื่นมาวางทับได้)

![image](https://github.com/user-attachments/assets/e9c50d69-006a-4830-a6a7-e716a53b5332)
 
รูปที่ 3.50 ผลที่ได้

3.3.35 กรณีที่ต้องการเอาอุปกรณ์แต่ละตัวมาเก็บไว้เพื่อให้ง่ายต่อการใช้งานครั้งถัดไปโดยไม่ต้องไปค้นหาอีกสามาถทำได้โดยการ Clone ดังรูป

![image](https://github.com/user-attachments/assets/91c6602b-410f-4725-b071-c18a73d6bd98)
 
รูปที่ 3.51 การเอาอุปกรณ์แต่ละตัวมาเก็บไว้เพื่อให้ง่ายต่อการใช้งานครั้งถัด

3.3.36 ตั้งชื่ออุปกรณ์ที่ Clone มา เพื่อให้แสดงในรายการของตนเอง (My Libraries)

![image](https://github.com/user-attachments/assets/be44c7c2-695f-4895-ae54-026c61f682ae)

รูปที่ 3.52 ตั้งชื่ออุปกรณ์ที่ Clone

3.3.37 เมื่อดูในไลบรารี่ของผู้ใช้งานจะเห็นรายการอุปกรณ์ที่ทำการ Clone เก็บเข้ามา
    (1) คลิกที่ี Work Space
    (2) คลิกที่ My Libraries->All จะเห็นรายการอุปกรณ์ (3)

![image](https://github.com/user-attachments/assets/bf8ae921-9e6c-4ab0-a32a-9f1fa9854669)

 
รูปที่ 3.53 เมื่อดูในไลบรารี่ของผู้ใช้งานจะเห็นรายการอุปกรณ์ที่ทำการ Clone

3.3.38 ดำเนินการต่อวงจร
    - ย้ายไปยังตำแหน่งที่เหมาะสม
    - เพิ่มอุปกรณ์ให้ครบ สามารถใช้การคัดลอกและวางอุปกรณ์ที่เหมือนกันที่เคยวางมาก่อนหน้านี้แล้ว
    - วางกราวด์ เลือกจากปุ่มกราวด์ (1)
    - ลากสายเชื่อมต่อวงจร เริ่มจากคลิกที่เครื่องมือเชื่อมต่อสายแล้วคลิกที่ขาอุปกรณ์เพื่อเชื่อมต่ออุปกรณ์เข้าด้วยกัน (2)

![image](https://github.com/user-attachments/assets/79764c5f-fa2e-41fd-a889-0a2c15d03dce)

 
รูปที่ 3.54 ดำเนินการต่อวงจร

3.3.39 เมื่อต่อวงจรเสร็จ ให้ทำการบันทึกไฟล์ (SAVE) แล้วทำการคลิกที่ปุ่ม Convert Schematic to PCB

![image](https://github.com/user-attachments/assets/ca775ee1-511d-4c61-bfbd-7b3b890d8e54)

 
รูปที่ 3.55 ต่อวงจรเสร็จและการ(save)

3.3.40 จะได้ไฟล์ที่มีอุปกรณ์ที่แสดงเป็นรูปตัวถัง (Foot Print) พร้อมสาย Net ที่แสดงว่าขาแต่ละอุปกรณ์มีการเชื่อมต่อตัวไหนบ้าง

![image](https://github.com/user-attachments/assets/3267d915-389c-4f8f-9287-7c267398e632)

 
รูปที่ 3.56 จะได้ไฟล์ที่มีอุปกรณ์ที่แสดงเป็นรูปตัวถัง (Foot Print)

3.3.41 ตั้งค่าหน่วยการแสดงผลและค่ากริดดังรูป

![image](https://github.com/user-attachments/assets/d655d01e-d758-4003-b75a-ff4cbfd80e9a)

 
รูปที่ 3.57 ตั้งค่าหน่วยการแสดงผลและค่ากริดดังรูป

3.3.42 ดำเนินการดังนี้
    - ทำการเคลื่อนย้ายอุปกรณ์ไปยังตำแหน่งที่เหมาะสม (ที่คิดว่าการเดินลายทองแดงไม่ยาก)
    - วางรูยึด PCB

![image](https://github.com/user-attachments/assets/31d13ba9-33e6-4d35-b894-6c27051321d5)

 
รูปที่ 3.58 วางรูยึด PCB

3.3.43 ผลที่ได้

![image](https://github.com/user-attachments/assets/ea9152c3-515f-462c-a02c-5975c10fb297)

 
รูปที่ 3.59 ผลที่ได้

![image](https://github.com/user-attachments/assets/88267fc5-fb07-4f4c-8dcb-739cf4450cd8)

 
รูปที่ 3.60 แผ่นพีซีบีสำหรับรถหุ่นยนต์

3.3.44 ทดลองแสดงผล 3 มิติ

 ![image](https://github.com/user-attachments/assets/22481cc9-15c6-41f6-b8a2-f89c9e89e9d0)

รูปที่ 3.61 แผ่นพีซีบีสำหรับรถหุ่นยนต์ 3 มิติ
