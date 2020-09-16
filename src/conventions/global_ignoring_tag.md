# Global Ignoring Tag (`***`)

## เกี่ยวกับ

แบบแผนนี้เป็นวิธีการสำหรับดาต้าแพ็คในการสื่อสารกับดาต้าแพ็คอื่นๆ โดยระบุ Tag entity ที่ดาต้าแพ็คอื่นๆสามารถใช้หากันได้ ข้อนี้จะใช้งานหนักที่สุดเพื่อป้องกันเหตุไม่คาดคิดของดาต้าแพ็คอื่นๆจากการ /kill หรือ ลบ entity ที่ summon เพื่อใช้งานพิเศษบางอย่าง เพราะว่าบางกรณีดาต้าแพ็คของคนๆนึงมีจังหวะที่ไม่ต้องการให้เกิดการยุ้งกับเอ็นทิตี้ดังกล่าว จึงต้องใช้แท็กนี้กันเอาไว้ เช่นในกรณีที่ ดาต้าแพ็คนึงสั่งซัมม่อน Armor stand ด้วย แท็กอะไรสักอย่าง เพื่อตรวจเช็คว่าบล็อคนั้นคือบล็อคอะไร แต่แล้วจู่ๆดาต้าแพ็คของอีกคนดันทำงาน /kill พอดีโดย สิ่งที่จะเกิดขึ้นคือ มันยังไม่ทันตรวจบล็อคดังกล่าวก็โดนดาต้าแพ็คของอีกคน /kill ไปสะก่อน บัคจึงบังเกิดขึ้นนั่นเอง

ในตอนนี้มีแท็ก ยกเว้น 4 แท็ก ดังนี้: `global.ignore`, `global.ignore.pos`, `global.ignore.gui`, `global.ignore.kill`

### 1. `global.ignore.kill`

เอ็นทิตี้ใดๆทีมีเจ้าแท็กนี้อยู่ **จะต้องไม่** ถูก ฆ่า โดยดาต้าแพ็คอื่นๆ แต่ไม่ได้จำกัดใช้แค่ `/kill` นะ
```mcfunction
execute as @e[type=creeper, tag=!global.ignore, tag=!global.ignore.kill] run kill @s
kill @e[type=creeper, tag=!global.ignore, tag=!global.ignore.kill]
```

### 2. `global.ignore.gui`

เอ็นทิตี้ใดๆที่มีแท็กนี้ **จะต้องไม่** แสดงผลเอฟเฟกต์ใดๆที่รอบๆเอ็นทิตี้นี้เลย แต่ไม่ได้จำกัดใช้แค่ `/title` นะ
```mcfunction
execute as @a[tag=!global.ignore.gui] at @s run title @s actionbar [{"text": "Hello, World!", "color": "green"}]
```
> ข้อนี้ยกเว้นกับคำสั่ง `/playsound`, `/tellraw` และ `/particle`

### 3. `global.ignore.pos`

เอ็นทิตี้ใดๆที่มีแท็กนี้ **จะต้องไม่** ถูกเคลื่อนย้าย จากจุดที่มันอยู่แม้แต่นิดเดียว แต่ไม่ได้จำกัดใช้แค่ `/tp`, `/teleport` นะ
```mcfunction
execute as @e[type=witch, tag=!global.ignore, tag=!global.ignore.pos] at @s run tp @s ~ ~0.1 ~
tp @e[type=area_effect_cloud, tag=!global.ignore, tag=!global.ignore.pos] ~ ~0.1 ~
```

### 4. `global.ignore`

เอ็นทิตี้ใดๆที่มีแท็กนี้ (รวมไปถึงผู้เล่นด้วย) **จะต้องไม่ถูก** execute @ selector เลย แท็กนี้คิดสะว่ามันเป็นแท็กรวมของการ ignore ทั้งหมดข้างต้น
```mcfunction
execute as @e[tag=!global.ignore] at @s run function namespace:internal/logic/function
```

> แท็กนี้ไม่สามารถใช้กับ selector สำหรับผู้เล่นเท่านั้นได้ (`@a`, `@e[type=player]`, `@p`, เป็นต้น.)  

## หมายเหตุ

แบบแผนนี้ใช้เฉพาะในกรณีที่ฟังก์ชั่นจะมีการ Modify NBT ของเอ็นทิตี้ที่ไม่รู้จัก หากคุณ Modify NBT ของเอ็นทิตี้ที่คุณรู้จัก (เช่น เอ็นทิตี้ที่มีแท็กกำหนดอยู่) คุณไม่ต้องใช้วิธีนี้
```mcfunction
execute as @e[tag=est.modify_et] run data modify entity @s <...>
execute as @e[type=zombie,tag=!global.ignore] run data modify entity @s <...>
```
พูดง่ายๆก็คือถ้า selector modify แบบมีแท็กระบุชัดเจนก็ไม่ต้องระบุ ignore

หากคุณไม่ต้องการให้ดาต้าแพ็คของคนอื่นมายุ่งกับ some tag ก็ใส่แบบนี้ได้
```mcfunction
summon armor_stand ~ ~ ~ {Tags:['some.tag','global.ignore','global.ignore.kill','global.ignore.gui','global.ignore.pos']}
```
ในกรณีที่ เอ็นทิตี้ดังกล่าวมีการใช้งานแล้วจบภายใน 1 ติ๊ก หรือเอ็นทิตี้มีการคงอยู่ในโลกแค่ 1 ติ๊ก ก็ไม่จำเป็นต้องใส่ก็ได้ 
```mcfunction
summon area_effect_cloud ~ ~ ~ {Tags: ['test.aec'], Age: 0, Duration: 1}
execute at @s positioned ~ ~ ~ run summon area_effect_cloud ~ ~ ~ {Tags: ['test.aec.rd','global.ignore','global.ignore.pos'], Age: 0, Duration: 1}
```
แต่ในกรณีที่มีการรันตลอด อยู่ 1 ติ๊กแล้วตายวนซ้ำหลายติ๊ก คุณอาจจะใส่ไว้ได้หากคุณไม่สบายใจว่าในเสียวมิลลิวินาทีของ 1 ติ๊กจะเกิดการขัดจังหวะขึ้น