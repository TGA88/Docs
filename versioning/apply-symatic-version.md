
## Apply Symatic version กับ Deployment Environment 

 >  แนะนำให้อ่าน เรื่อง Symatic version and Pre-release ก่อน [Click](symatic-version.md)

 #### alpha version
ทดสอบเบื้องต้น ด้วย ci pipeline ซึ่งประกอบด้วย 
- basic code scan ประกอบด้วย 
    - unit-test
    - build code ผ่าน
    - lint ตรวจ syntax ว่าตรงตาม convention coding ของ team
- Test Db-migration script
> หาก ci รัน Stage ดังกล่วผ่าน จะทำการ Release Alpha version  


####  beta version
เมื่อ Team deploy group of alpha version (ระบบงาน อาจจะมี alpha version ของ app และ api server หลายตัว) ขึ้น QA Server สำเร็จ จะได้ beta version เพื่อให้ cd-pipeline รัน automation functional-test(integration/Component test) และ QA ทำการ manuel test ในส่วนที่ไม่สามารถ automateได้
> integration/Component test ในที่นี้ หมายถึง APP และ API ทุกตัวที่เกี่ยวข้องกัน ที่ Team เป็นเจ้าของ

#### rc version 
เมื่อ QA confirm ว่า Test beta ผ่าน แล้ว จะทำาร Promote beta เป็น rc version (ด้วย การ approve stage promote version to rc) และ จะ Deploy มาที่ UAT 
เพื่อให้ 
- ทีม BP ทำ Expolartory Test,System integration-Test 
- ทีม Security ทำ  Planetary-test (Nonfunctional-Test)  
- ทีม Infrastructure(หรือ ใครก็ตามที่ได้รับมอบหมายให้ทำ) ทำ Perfomance-test (Nonfunctional-Test)ประกอบด้วย
    - load-test,
    - stress-test
    - spike-test
    - volume-test
> **Perfomance-Test** ไม่จำเป็นต้องทำทุกประเภทให้เลือก เอาตามความเหมาะสม ของสถานะการ
**System-integration-test(Intenal Service)** ในที่นี้หมายถึง  การเทส component ที่Teamเราเป็นเจ้าของ กับ Service(Test Env) ที่ Team อื่น เป็นเจ้าของ (Team อื่นในที่นี้ จะยังคงเป็น Team ภายในบริษัทเดียวกันอยู่ ซึ่งน่าจะนยังมีความใกล้ชิดกันพอที่ จะทำ Test Server ให้เราใช้ทดสอบได้) **ถ้าเป็น External Service ซึ่งบริษัทอื่นเป็นเจ้าของ** เราคงต้องทำ mock server เอง ซึ่งส่วนมากการเทสระดับนี้เราจะไม่ได้ทำ เพราะ service ปลายทางไม่มีเครื่องทดสอบให้
### Stable version
เมื่อ BP confirm ว่า Functional Test ผ่าน ผู้ที่รับผิดชอบทำ Nonfunctional-test confirm ว่า ผ่าน BP จะทำการ Promoate Version Level จาก rc เป็น Stable เพื่อ Deloy ขึ้น PRD Server 