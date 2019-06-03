# เกี่ยวกับ Electron

[Electron](https://electronjs.org) เป็นโอเพนซอร์ซไลบรารีที่พัฒนาโดย GitHub เอาไว้ใช้สำหรับการพัฒนาเดสก์ท็อป ครอสแพลตฟอร์มแอปพลิเคชัน ด้วยภาษา HTML, CSS, และ JavaScript การทำงานของ Electron เกิดขึ้นจากการผสมผสานระหว่าง [Chromium](https://www.chromium.org/Home) และ [Node.js](https://nodejs.org) เข้าด้วยกัน และตัวแอปสามารถนำไปใช้ได้กับทั้งระบบปฏิบัติการ Mac, Windows, และ Linux

อิเล็กตรอนเริ่มขึ้นในปี 2556 เนื่องจากเป็นเฟรมเวิร์กที่จะสร้าง [ Atom ](https://atom.io) ซึ่งเป็นโปรแกรมแก้ไขข้อความที่แฮ็กของ GitHub ทั้งสองถูกเปิดแหล่งที่มาในฤดูใบไม้ผลิของปี 2014

มันได้กลายเป็นเครื่องมือยอดนิยมที่ใช้โดยนักพัฒนาโอเพ่นซอร์ส startups และ บริษัท ที่จัดตั้งขึ้น [ ดูว่าใครกำลังสร้างอิเล็กตรอนอยู่ ](https://electronjs.org/apps)

อ่านต่อเพื่อเรียนรู้เพิ่มเติมเกี่ยวกับผู้มีส่วนร่วมและปล่อยอิเล็กตรอนหรือเริ่มต้นสร้างด้วยอิเล็กตรอนใน  คู่มือการเริ่มต้นอย่างรวดเร็ว <0></p> 

## ทีมหลักและผู้สนับสนุน

อิเล็กตรอนได้รับการดูแลโดยทีมงานที่ GitHub รวมถึงกลุ่มของผู้มีส่วนร่วม [ ผู้มีส่วนร่วม ](https://github.com/electron/electron/graphs/contributors) จากชุมชน ผู้ให้ข้อมูลบางคนเป็นบุคคลและทำงานใน บริษัท ขนาดใหญ่ที่กำลังพัฒนาเกี่ยวกับอิเล็กตรอน เรายินดีที่จะเพิ่มผู้มีส่วนร่วมให้กับโครงการในฐานะผู้ดูแล อ่านเพิ่มเติมเกี่ยวกับ [ การมีส่วนร่วมกับอิเล็กตรอน ](https://github.com/electron/electron/blob/master/CONTRIBUTING.md)

## การเผยแพร่

[ อิเล็กตรอนปล่อย ](https://github.com/electron/electron/releases) บ่อยครั้ง เราปล่อยเมื่อมีการแก้ไขข้อบกพร่องที่สำคัญ API ใหม่หรือกำลังอัปเดตเวอร์ชันของ Chromium หรือ Node.js

### การอัพเดท

โดยปกติแล้ว Chromium รุ่นของอิเล็กตรอนจะอัปเดตภายในหนึ่งหรือสองสัปดาห์หลังจากรุ่น Chromium ที่เสถียรใหม่ออกวางจำหน่ายทั้งนี้ขึ้นอยู่กับความพยายามที่เกี่ยวข้องในการอัปเกรด

เมื่อมีการปล่อย Node.js เวอร์ชั่นใหม่อิเล็กตรอนมักจะรอประมาณหนึ่งเดือนก่อนที่จะอัปเกรดเพื่อให้มีเวอร์ชั่นที่เสถียรยิ่งขึ้น

ในอิเลคตรอน Node.js และ Chromium แบ่งปันอินสแตนซ์ V8 เดียวโดยปกติจะเป็นเวอร์ชันที่ Chromium ใช้อยู่ เวลาส่วนใหญ่ * นี้ใช้งานได้ * แต่บางครั้งก็หมายถึงการแก้ไข Node.js

### เวอร์ชั่น

ในฐานะที่เป็นรุ่น 2.0 อิเล็กตรอน [ ตามด้วย `semver` ](https://semver.org) สำหรับแอปพลิเคชันส่วนใหญ่และการใช้ npm เวอร์ชันล่าสุดใด ๆ การใช้ `$ npm install electron` จะทำสิ่งที่ถูกต้อง

กระบวนการอัปเดตเวอร์ชันนี้มีรายละเอียดอย่างชัดเจนใน [Versioning Doc](electron-versioning.md).

### LTS

การสนับสนุนระยะยาวของอิเล็กตรอนรุ่นเก่าไม่มีอยู่ในปัจจุบัน หากอิเล็กตรอนรุ่นปัจจุบันของคุณทำงานให้คุณคุณสามารถอยู่กับมันได้นานเท่าที่คุณต้องการ หากคุณต้องการใช้ประโยชน์จากฟีเจอร์ใหม่ ๆ เมื่อเข้ามาคุณควรอัพเกรดเป็นเวอร์ชั่นใหม่

อัปเดตที่สำคัญมาพร้อมกับเวอร์ชัน ` v1.0.0 ` หากคุณยังไม่ได้ใช้เวอร์ชันนี้คุณควร [ อ่านเพิ่มเติมเกี่ยวกับ ` v1.0.0 ` การเปลี่ยนแปลง ](https://electronjs.org/blog/electron-1-0)

## Core Philosophy

เพื่อให้อิเล็กตรอนมีขนาดเล็ก (ขนาดไฟล์) และยั่งยืน (การแพร่กระจายของการพึ่งพาและ API) โครงการ จำกัด ขอบเขตของโครงการหลัก

ตัวอย่างเช่น Electron ใช้ไลบรารีการแสดงผลของ Chromium มากกว่า Chromium ทั้งหมด สิ่งนี้ทำให้การอัปเกรด Chromium ง่ายขึ้น แต่ยังหมายถึงคุณลักษณะบางอย่างของเบราว์เซอร์ที่พบใน Google Chrome ไม่มีอยู่ในอิเล็กตรอน

คุณสมบัติใหม่ที่เพิ่มเข้ากับอิเล็กตรอนควรเป็น API ดั้งเดิม หากคุณสมบัติสามารถเป็นโมดูล Node.js ของตัวเองก็น่าจะเป็น ดู [ เครื่องมืออิเล็กตรอนที่สร้างโดยชุมชน ](https://electronjs.org/community)

## ประวัติ

ด้านล่างเป็นเหตุการณ์สำคัญในประวัติศาสตร์ของอิเล็กตรอน

| :calendar:       | :tada:                                                                                                            |
| ---------------- | ----------------------------------------------------------------------------------------------------------------- |
| **เมษายน 2013**  | [ Atom Shell เริ่มต้นขึ้น ](https://github.com/electron/electron/commit/6ef8875b1e93787fa9759f602e7880f28e8e6b45) |
| **พฤษภาคม 2014** | [ Atom Shell เปิดแหล่งที่มา ](https://blog.atom.io/2014/05/06/atom-is-now-open-source.html)                       |
| **เมษายน 2015**  | [ Atom Shell ได้รับการตั้งชื่อใหม่ว่าอิเล็กตรอน ](https://github.com/electron/electron/pull/1389)                 |
| **พฤษภาคม 2016** | [ Electron ปล่อย `v1.0.0` ](https://electronjs.org/blog/electron-1-0)                                             |
| **พฤษภาคม 2016** | [ แอปอิเล็กตรอนที่เข้ากันได้กับ Mac App Store ](mac-app-store-submission-guide.md)                                |
| **สิงหาคม 2016** | [ การสนับสนุน Windows Store สำหรับแอป Electron ](windows-store-guide.md)                                          |