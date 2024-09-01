# Rolling Back Actions with git reflog

## บทนำ

`git reflog` เป็นเครื่องมือที่มีประสิทธิภาพในการดูประวัติของการกระทำที่เกิดขึ้นใน Git repository โดย `git reflog` ช่วยให้คุณสามารถย้อนกลับไปยังสถานะก่อนหน้าที่ต้องการได้ เช่น เมื่อคุณทำการ reset หรือ commit ที่ไม่ตั้งใจ

## ขั้นตอนการย้อนกลับการกระทำ (Rollback)

### 1. การตรวจสอบประวัติการกระทำด้วย `git reflog`

ก่อนที่คุณจะสามารถย้อนกลับการกระทำใด ๆ ได้ คุณต้องใช้ `git reflog` เพื่อดูว่ามีการกระทำอะไรเกิดขึ้นบ้างใน repository ของคุณ:

```bash
git reflog
```

ตัวอย่างผลลัพธ์:

```
4360f5b (HEAD -> dev) HEAD@{0}: reset: moving to HEAD
4360f5b (HEAD -> dev) HEAD@{1}: reset: moving to HEAD~1
976ccfb (origin/kurosos-devka, origin/dev, kurosos-devka) HEAD@{2}: reset: moving to HEAD
976ccfb (origin/kurosos-devka, origin/dev, kurosos-devka) HEAD@{3}: merge kurosos-devka: Fast-forward
```

### 2. การระบุจุดที่ต้องการย้อนกลับ

จากผลลัพธ์ `git reflog` ให้ดูที่ commit hash และตำแหน่งของ `HEAD` ที่คุณต้องการย้อนกลับไป ตัวอย่างเช่น หากคุณต้องการกลับไปยัง commit `976ccfb` ที่อยู่ที่ `HEAD@{2}`

### 3. การย้อนกลับไปยัง commit ที่ต้องการ

ใช้คำสั่ง `git reset --hard` เพื่อย้อนกลับไปยัง commit ที่คุณต้องการ:

```bash
git reset --hard 976ccfb
```

แทนที่ `976ccfb` ด้วย hash ของ commit ที่คุณต้องการย้อนกลับไป คำสั่งนี้จะรีเซ็ต repository ของคุณกลับไปยังสถานะของ commit นั้น และจะลบการเปลี่ยนแปลงทั้งหมดที่ไม่ได้ commit ไว้

### คำเตือนในการใช้ `git reset --hard`

- **ลบการเปลี่ยนแปลงที่ยังไม่ได้บันทึก**: `git reset --hard` จะลบการเปลี่ยนแปลงทั้งหมดที่ยังไม่ได้ commit ดังนั้นข้อมูลอาจสูญหายได้ถ้าไม่ได้ commit การเปลี่ยนแปลงไว้
- **ใช้ด้วยความระมัดระวัง**: ก่อนใช้คำสั่งนี้ ควรตรวจสอบให้แน่ใจว่าคุณต้องการย้อนกลับไปยัง commit นั้นจริง ๆ และไม่มีการเปลี่ยนแปลงที่ยังไม่ได้บันทึกที่สำคัญ

## ตัวอย่างการใช้งานจริง

หากคุณทำการ reset ไปที่ commit ล่าสุด และพบว่า commit นั้นไม่ถูกต้อง คุณสามารถใช้ `git reflog` เพื่อค้นหา commit ที่ถูกต้อง แล้วใช้คำสั่ง `git reset --hard` เพื่อย้อนกลับไปยัง commit ที่ต้องการ

ตัวอย่าง:

1. ใช้ `git reflog` เพื่อค้นหา commit ก่อนหน้า
2. เมื่อพบ commit ที่ต้องการแล้ว เช่น `976ccfb` ให้ใช้คำสั่ง:

   ```bash
   git reset --hard 976ccfb
   ```

## สรุป

การใช้ `git reflog` ร่วมกับ `git reset --hard` ช่วยให้คุณสามารถย้อนกลับไปยังสถานะก่อนหน้าของ repository ได้อย่างง่ายดายและรวดเร็ว โดยเฉพาะเมื่อเกิดข้อผิดพลาดที่ไม่ตั้งใจ