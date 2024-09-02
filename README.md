---Strapi คือ ระบบ headless CMS(Content Management System) ที่จะแยกการทำงานของ frontend backend database ออกจากกันทำให้สามารถทำงานในแต่ละส่วนได้ในเวลาเดียวกัน ช่วยสร้างและจัดการ API สำหรับโปรเจคได้ง่ายขึ้น โดยใช้ nodejs เป็นฐานหลัก 
โดยStrapi เป็นหนึ่งในโปรแกรม headless CMS ที่ใช้งานง่าย รวดเร็ว มีความปลอดภัย ทำงานร่วมกับ framework รุ่นใหม่ๆได้อย่างดี
(อ้างอิง https://morphos.is/th/blog/what-is-strapi-and-how-it-will-dominate-the-world-of-headless-cms)

---.gitignore คือ ไฟล์ที่ใช้ในระบบควบคุมเวอร์ชันของ git เพื่อระบุไฟล์ที่ git ไม่ทำการ commit หรือจัดเก็บใน repository 
ใช้งานโดยการสร้างไฟล์ .gitignore และระบุไฟล์หรือโฟลเดอร์ที่ไม่ต้องการ commit ลงไปไว้ในไฟล์ .gitignore 

--------------------ขั้นตอนการติดตั้ง Strapi--------------------
1.สร้างโฟลเดอร์ไว้ยัง directory ที่เราต้องการจะสร้างโปรเจค
2.ติดตั้งเครื่องมือต่างๆที่จำเป็นสำหรับการทำงานของ Strapi
	-nodejs(เวอร์ชันที่เป็นเลขคู่ เช่น v18, v20)
	-npm(เวอร์ชัน 6 ขึ้นไป)
3.ใช้ command prompt หรือ terminal ทำการ cd ไปยังโฟลเดอร์ที่สร้างไว้ในขั้นตอนที่ 1 แล้วใช้คำสั่งติดตั้ง Strapi
	-npx create-strapi-app@latest my-project  (my-project สามารถเปลี่ยนเป็นชื่อโปรเจคตามที่ต้องการได้)
4.เลือกประเภทการติดตั้งระหว่าง quickstart และ custom โดยการเลือก custom จะทำให้สามารถเลือกฐานข้อมูลที่ต้องการได้
5.หลังจากติดตั้ง Strapi แล้วสามารถ cd เข้าไปในโปรเจคที่สร้างแล้วใช้คำสั่ง npm run develop และเริ่มการทำงานของ Strapi
*****สามารถทดสอบการติดตั้งว่าสำเร็จหรือไม่โดย เมื่อใช้คำสั่ง npm run develop แล้วให้ลองเข้าไปตาม URL ที่ Strapi แสดงขึ้นมา

--------------------ขั้นตอนการนำ Strapi ขึ้นไปไว้บน github--------------------
1.ทำการสร้าง repository เพื่อไว้เก็บ Strapi

***หากเลือกสร้างไฟล์ readme ตอนสร้าง repository***
2.ทำการ clone repository ไปไว้ใน directory ที่ต้องการ โดย
	-git clone "URL ของ repositoty"
3.ทำการนำไฟล์โปรเจค Strapi มาไว้ในไฟล์ repository ที่ clone มาจากขั้นตอนก่อนหน้า
4.ใช้ command prompt หรือ terminal ทำการ cd ไปยังไฟล์ repository ที่ clone มา
5.ทำการ add ไฟล์ทั้งหมดโดย
	-git add .
6.ทำการ commit
	-git commit -m "ตัวควบคุมเวอร์ชัน"
7.ทำการ push 
	-git push origin

***หากไม่ได้สร้างไฟล์ readme ตอนสร้าง repository***
2.ใช้ command prompt หรือ terminal ทำการ cd ไปยัง directory ที่โปรเจค Strapi อยู่
3.ใช้คำสั่ง git init
4.ทำการ add ไฟล์ทั้งหมดโดย
	-git add .
5.ทำการ commit
	-git commit -m "ตัวควบคุมเวอร์ชัน"
6.ทำการตั้ง remote ไปยัง repository ของ GitHub ที่ต้องการ
	-git remote add origin "URL ของ repository ที่ต้องการเก็บ Strapi"
7.ทำการ push
	-git push -u origin "branch ที่ต้องการ"

--------------------ขั้นตอนการติดตั้ง Strapi จาก GitHub ลงบน Amazon EC2--------------------
1.สร้าง instance โดยเลือกใช้เป็นประเภท t2.small และสร้าง key.pem เพื่อใช้เชื่อมต่อ
2.ตั้งค่า Security ของ instance เพื่อให้สามารถใช้ UI browser ของเครื่องในการเข้าถึง URL ของ Strapi ที่ทำงานบน EC2 instance ได้
	-ตรวจสอบ security group name ของ instance 
	-ไปที่แท็บ Security Group
	-ทำการ add rule ไปยัง inbound rule ของ security group name นั้นโดยมีคุณสมบัติคือ
		-Type: Custom TCP
		-Protocol: TCP
		-Port Range: 1337(portที่จะใช้เพื่อเข้าถึงเว็ปของ Strapi)
		-source: 0.0.0.0/0(เพื่อให้เข้าถึงได้อย่างอิสระ)
3.ทำการเชื่อมต่อ command prompt หรือ terminal กับ EC2 instance โดยใช้วิธี SSH Tunneling เพื่อให้เครื่องใดๆในอินเตอร์เน็ตนี้เข้าถึง port 1337 ได้
	-ssh -L 1337:localhost:1337 -i "keyที่สร้างมากับinstance" "username และ IP address ของ EC2 instance" เช่น "ssh -L 1337:localhost:1337 -i strapiProject.pem ec2-user@ec2-3-232-133-238.compute-1.amazonaws.com"
4.ทำการติดตั้งเครื่องมือต่างๆที่จำเป็นลงเป็น EC2 instance 
	-sudo yum update -y
	-sudo yum install -y nodejs (ติดตั้ง nodejs)
	-sudo yum install -y git (ติดตั้ง git)
	-sudo npm install -g npm@10.8.3 (อัปเดต npm)
	-sudo groupinstall 'Development Tools' -y
5.ทำการ clone โปรเจค Strapi มาจาก GitHub
	-git clone "URL ของ GitHub ที่เก็บ Strapi"
6.cd เข้าไปยัง repository ที่cloneมา แล้วใช้คำสั่งติดตั้ง dependencies
	-npm install
7.ทำการ update npm
	-npm update
8.ตั้งค่า key ในไฟล์ config/server.js
	-โดยในส่วนของ app ให้เพิ่มkeyไปสองตัว เช่น
		-app:{
		    key: env.array('APP_KEYS',['myKeyA', 'myKeyB']
		 }
9.ทำการตั้ง Token ข้อง auth และ api โดยเจน Token มาสอง Token
	-node -e "console.log(require('crypto').randomBytes(16).toString('base64'))"
	-แล้วนำแต่ละ Token ไปใส่ใน config/admin.js
		-auth: {
    			secret: env('ADMIN_JWT_SECRET', "Token ที่เจนมา"),
 		 },
		 apiToken: {
    			salt: env('API_TOKEN_SALT', "Token ที่เจนมา"),
  		 },
10.ลองเริ่มการทำงานของ Strapi
	-npm run develop



