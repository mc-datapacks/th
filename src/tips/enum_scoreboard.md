# Enum Scoreboard

คุณสามารถใช้ Fake player ในการเก็บค่าอะไรบางอย่างใน scoreboard ได้ แทนการใช้ตัวเลขโดยตรงในเงื่อนไข มันจะทำให้โค๊ดของคุณดูละเอียดมากขึ้น บอกหน้าที่ได้มากขึ้น และเพิ่มความสามารถในการปรับปรุงแก้ไขซ่อมแซมโค๊ดได้ง่ายในอนาคต

## ประกาศตัวแปร Enum

```
scoreboard players set #state.idle bb.enum 1
scoreboard players set #state.foo bb.enum 2
scoreboard players set #state.bar bb.enum 3
```
Enum สามารถเก็บค่าใดๆที่คุณต้องการได้ ไม่ว่าจะเป็น ค่าคงที่บางอย่าง หรือ ค่าที่ใช้ครั้งเดียวแล้ว reset รับค่าใหม่เพราะมีการรับค่าตรวจสอบและเปลี่ยนแปลงตลอดเวลา ซึ่งการใช้งานนี้เรารู้อยู่แล้วว่า Fake player ใช้งานอะไรและมันมีอยู่ตลอดเวลา แล้วแต่จะนำไปใช่ มันจะหายไปก็ต่อเมื่อมีการ reset และเซ็ตกลับมาใหม่ได้ จะสังเกตเห็นว่า # Fakeplayer จะไม่โชว์ขึ้นมาที่ sidebar เพราะเป็นการใช้งานเฉพาะที่ซ่อนอยู่เบื้องหลัง

## ตัวอย่าง ใช้ Enum เช็ค State

```
execute if score @s bb.state = #state.idle bb.enum run function <idle_state>
execute if score @s bb.state = #state.running bb.enum run function <running_state>
execute if score @s bb.state = #state.stopping bb.enum run function <stopping_state>
```
วิธีนี้ทำให้เราตรวจสอบ สถานะค่าบางอย่างของ `@s` ได้ เมื่อตรงตามเงื่อนไขเราก็ทำให้มันเรียกทำงาน function เฉพาะของเหตุการณ์นั้นได้
