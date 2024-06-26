# OWASP-10-Server-Side-Request-Forgery-SSRF-

  การโจมตีทางความปลอดภัยในเว็บแอปพลิเคชันที่ผู้โจมตีสามารถบังคับเซิร์ฟเวอร์ของแอปพลิเคชันให้ส่งคำขอ HTTP ไปยังปลายทางที่เลือกได้ ผู้โจมตีสามารถใช้การโจมตีนี้เพื่อ

  1.  เข้าถึงข้อมูลที่ไม่ควรเข้าถึง: เช่น การเข้าถึงข้อมูลภายในของระบบหรือเครือข่ายที่ไม่สามารถเข้าถึงได้จากภายนอก

  2.  สแกนและโจมตีเครือข่ายภายใน: ใช้เซิร์ฟเวอร์ของแอปพลิเคชันเพื่อสแกนเครือข่ายภายในหรือโจมตีระบบอื่นในเครือข่ายเดียวกัน

  3.  Bypass firewalls: ทำให้สามารถส่งคำขอผ่านไฟร์วอลล์ที่ควรจะป้องกันไม่ให้เกิดการเชื่อมต่อจากภายนอก

  4.  นำไปสู่การโจมตีอื่น ๆ: เช่น การโจมตี Remote Code Execution (RCE) หรือการโจมตีแบบ Data Exfiltration

วิธีการทำงานของ SSRF:

  เมื่อแอปพลิเคชันรับ URL จากผู้ใช้และส่งคำขอ HTTP ไปยัง URL นั้นโดยไม่มีการตรวจสอบที่เพียงพอ ผู้โจมตีสามารถเปลี่ยน URL ที่ส่งมาเพื่อบังคับให้เซิร์ฟเวอร์ทำการร้องขอไปยังที่อยู่ที่ผู้โจมตีควบคุมหรือที่อยู่ภายในระบบ

การป้องกัน SSRF:

  1.  การตรวจสอบและกรองอินพุต: ตรวจสอบและกรอง URL ที่รับจากผู้ใช้

  2.  การใช้ Allowlist: อนุญาตเฉพาะ URL หรือโดเมนที่รู้จักและเชื่อถือได้เท่านั้น

  3.  การจำกัดการเชื่อมต่อภายใน: ไม่อนุญาตให้เซิร์ฟเวอร์เชื่อมต่อกับที่อยู่ภายในที่ไม่จำเป็น

  4.  การตรวจสอบและจำกัดการออกไปภายนอก: ตรวจสอบและจำกัดการเชื่อมต่อจากเซิร์ฟเวอร์ออกไปภายนอกอย่างเข้มงวด

Cross-Site Request Forgery (CSRF)

  การโจมตีทางเว็บแอปพลิเคชันที่ผู้โจมตีสามารถบังคับให้ผู้ใช้ที่ได้รับการรับรองแล้วส่งคำขอ HTTP ที่ไม่พึงประสงค์ไปยังแอปพลิเคชันที่เชื่อถือได้ ซึ่งอาจส่งผลให้เกิดการกระทำที่ผู้ใช้ไม่ต้องการในระบบ เช่น การเปลี่ยนรหัสผ่าน การทำธุรกรรม หรือการส่งข้อมูลสำคัญ

วิธีการทำงานของ CSRF:

  1.  ผู้ใช้เข้าสู่ระบบ: ผู้ใช้เข้าสู่ระบบในเว็บแอปพลิเคชัน (เช่น เว็บธนาคาร) และเบราว์เซอร์ของผู้ใช้เก็บข้อมูลการเข้าสู่ระบบ (เช่น คุกกี้เซสชัน) ไว้

  2.  ผู้ใช้เข้าชมเว็บไซต์ที่ไม่ปลอดภัย: ผู้ใช้คลิกที่ลิงก์หรือเข้าชมหน้าเว็บที่ควบคุมโดยผู้โจมตี

  3.  ผู้โจมตีสร้างคำขอปลอม: เว็บไซต์ของผู้โจมตีสร้างคำขอ HTTP ที่จะถูกส่งไปยังแอปพลิเคชันที่เชื่อถือได้ โดยใช้ข้อมูลการเข้าสู่ระบบของผู้ใช้จากเบราว์เซอร์

  4.  แอปพลิเคชันเชื่อถือคำขอ: แอปพลิเคชันที่เชื่อถือได้รับคำขอและดำเนินการตามคำขอเนื่องจากคิดว่ามาจากผู้ใช้ที่ได้รับการรับรอง

การป้องกัน CSRF:

  1.  ใช้ CSRF Token: แอปพลิเคชันควรสร้างโทเค็น CSRF ที่ไม่ซ้ำและสุ่มสำหรับแต่ละเซสชันและรวมไว้ในทุกคำขอที่ส่งไปยังเซิร์ฟเวอร์ จากนั้นตรวจสอบโทเค็นนี้ในเซิร์ฟเวอร์ว่าตรงกับโทเค็นที่เก็บไว้

  2.  ใช้ SameSite Cookie Attribute: กำหนดค่า SameSite attribute สำหรับคุกกี้เป็น Strict หรือ Lax เพื่อป้องกันไม่ให้เบราว์เซอร์ส่งคุกกี้ในคำขอข้ามไซต์

  3.  ยืนยันการกระทำที่สำคัญ: ขอให้ผู้ใช้ยืนยันการกระทำที่สำคัญอีกครั้ง เช่น การป้อนรหัสผ่านหรือใช้การยืนยันสองขั้นตอน (2FA)

  4.  ตรวจสอบ Origin และ Referer Header: ตรวจสอบ Origin และ Referer headers ของคำขอเพื่อให้แน่ใจว่ามาจากไซต์ที่เชื่อถือได้

Cross-Site Request Forgery (CSRF)

การปลอมแปลงคำขอข้ามไซต์ คือการโจมตีรูปแบบหนึ่งที่มุ่งเน้นไปที่การใช้ประโยชน์จากการตรวจสอบสิทธิ์ของผู้ใช้ที่ได้รับการยืนยันโดยเว็บไซต์หนึ่ง แล้วบังคับให้เบราว์เซอร์ของผู้ใช้ส่งคำขอที่ไม่พึงประสงค์ไปยังเว็บไซต์นั้น

ลักษณะของการโจมตี CSRF:

  1.  การเข้าสู่ระบบ: ผู้โจมตีต้องรู้หรือเดาได้ว่าผู้ใช้กำลังเข้าสู่ระบบในเว็บไซต์ที่เป็นเป้าหมาย

  2.  การส่งคำขอปลอม: ผู้โจมตีสร้างคำขอ HTTP (เช่น การเปลี่ยนแปลงข้อมูล การทำธุรกรรม) ที่ผู้ใช้ได้รับการยืนยันแล้วสามารถทำได้

  3.  การดำเนินการโดยไม่รู้ตัว: ผู้ใช้คลิกที่ลิงก์หรือเข้าชมหน้าเว็บที่ฝังคำขอปลอมไว้โดยไม่รู้ตัว

วิธีป้องกันการโจมตี CSRF:

  1.  ใช้โทเค็น CSRF (CSRF Token): ใส่โทเค็นที่ไม่ซ้ำกันในฟอร์มและตรวจสอบโทเค็นนี้ในฝั่งเซิร์ฟเวอร์เมื่อได้รับคำขอ

  2.  การตรวจสอบและตั้งค่า Referer Header: ตรวจสอบ Referer Header ในคำขอเพื่อยืนยันว่ามาจากโดเมนที่ถูกต้อง

  3.  การตั้งค่า SameSite Cookie: ตั้งค่า SameSite attribute ของคุกกี้เพื่อป้องกันการส่งคุกกี้ไปยังเว็บไซต์อื่น

  4.  การใช้หลักการ Post-Redirect-Get (PRG): ใช้การรีไดเรกต์หลังจากการโพสต์เพื่อป้องกันการทำซ้ำคำขอโดยไม่ตั้งใจ

Server-Side Request Forgery (SSRF)

  การปลอมแปลงคำขอฝั่งเซิร์ฟเวอร์ คือการโจมตีที่ผู้โจมตีทำให้เซิร์ฟเวอร์ส่งคำขอ HTTP ไปยังที่อยู่ที่ผู้โจมตีเลือก โดยการโจมตีนี้มักมุ่งเน้นไปที่การใช้ประโยชน์จากการที่เซิร์ฟเวอร์สามารถส่งคำขอไปยังบริการหรือโดเมนอื่นได้ เช่น การเข้าถึงข้อมูลที่ไม่ได้เปิดเผยสู่ภายนอก หรือการใช้เซิร์ฟเวอร์เป็นทางผ่านเพื่อโจมตีระบบภายในเครือข่าย

ลักษณะของการโจมตี SSRF:

  1.  การส่งคำขอจากเซิร์ฟเวอร์: ผู้โจมตีใช้ช่องโหว่ในเว็บแอปพลิเคชัน เพื่อบังคับให้เซิร์ฟเวอร์ส่งคำขอ HTTP ไปยังที่อยู่ URL ที่ผู้โจมตีควบคุมหรือที่อยู่ภายในเครือข่ายภายในของเซิร์ฟเวอร์

  2.  การเข้าถึงข้อมูลที่ไม่ได้รับอนุญาต: ผู้โจมตีสามารถเข้าถึงข้อมูลที่ไม่สามารถเข้าถึงได้จากภายนอก เช่น การดึงข้อมูลจากเซิร์ฟเวอร์อื่นภายในเครือข่าย หรือการเข้าถึงบริการที่ไม่ได้เปิดเผยต่อภายนอก

  3.  การโจมตีภายในเครือข่าย: ผู้โจมตีอาจใช้ SSRF เพื่อทำการโจมตีต่อเนื่อง เช่น การทำ port scanning ภายในเครือข่าย การโจมตีช่องโหว่ของบริการภายใน หรือการเข้าถึงฐานข้อมูล

วิธีป้องกันการโจมตี SSRF:

  1.  การตรวจสอบและกรอง URL: ตรวจสอบและกรอง URL ที่ผู้ใช้สามารถป้อนเข้ามา เช่น ห้ามไม่ให้ผู้ใช้ส่ง URL ที่ชี้ไปยัง IP ภายในเครือข่าย หรือที่อยู่ภายใน (localhost, 127.0.0.1)

  2.  การจำกัดการเข้าถึงเครือข่ายภายใน: จำกัดสิทธิ์ของเซิร์ฟเวอร์ในการเข้าถึงเครือข่ายภายในและบริการที่ไม่จำเป็น

  3.  การใช้รายการที่อนุญาต (Allowlist): ใช้รายการที่อนุญาตเฉพาะ URL หรือโดเมนที่เชื่อถือได้และจำเป็นสำหรับการทำงานของแอปพลิเคชัน

  4.  การตรวจสอบและบันทึกคำขอ: ตรวจสอบและบันทึกคำขอที่ส่งออกจากเซิร์ฟเวอร์เพื่อให้สามารถตรวจจับพฤติกรรมที่ผิดปกติได้

  5.  การใช้ Proxy หรือ Firewall: ใช้ Proxy หรือ Firewall เพื่อตรวจสอบและควบคุมการรับส่งข้อมูลออกจากเซิร์ฟเวอร์

Video การทำ Cross-Site Request Forgery (CSRF) : https://www.youtube.com/watch?v=ZU_37M_1xg8

Video การทำ Server-Side Request Forgery (SSRF) : https://www.youtube.com/watch?v=ccCP4STs968
