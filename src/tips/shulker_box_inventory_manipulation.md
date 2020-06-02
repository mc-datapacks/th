# จัดการช่องเก็บของของผู้เล่นด้วย Shulker Box

## คืออะไร
นี่คือเทคนิคติดตามเพื่อจัดการกับช่องเก็บของของผู้เล่น เพียงแค่ใช้ระบบ loot table ของ NBT Block entity Data Shulker Box 

## หลักการ
เราใช้ประโยชน์จากข้อเท็จจริงที่ว่า Shulker Box สามารถทำให้มันดรอปของในกล่องบนพื้นได้แทนที่จะเป็นตัวกล่องที่มีไอเทมอยู่ข้างในซึ่งแนวคิดนี้สามารถใช้ คำสั่ง `/loot` เพื่อแทนที่หรือแก้ไขช่องเก็บของของผู้เล่นได้

## การใช้งานคร่าวๆ

1\. ในการปฏิบัติตาม [แบบแผน ทางการ](/conventions/index.md) คุณต้องแก้ไข Loot table ของ `minecraft:yellow_shulker_box` ให้เป็น Loot table ตามที่เขียนไว้ Loot table ดังกล่าวจะนำ ไอเทมออกจาก Shulker Box โดยไม่ติด Shulker Box มาด้วยมีแค่ไอเทมจาก Shulker Box เท่านั้น เมื่อมีการขุดโดยอุปกรณ์ที่มี NBT `{drop_contents: 1b}`  
[<center>https://pastebin.com/4sspBvep</center>](https://pastebin.com/4sspBvep)

2\. สร้างกล่อง Shulker Box เฉพาะขึ้นมาเพื่อจัดการช่องเก็บของของผู้เล่น โดยปกติมันกล่องจะวางห่างจากสายตาของผู้เล่น หรือในพื้นที่ๆไม่มีการใช้งาน โดยปกติทำกันที่ `~ 255 ~` จากพิกัดของผู้เล่นหรือ เอ็นทิตี้ใดๆ แต่เพื่อเป็นตัวอย่างจึงขอใช้ `~ ~ ~`

```
setblock ~ ~ ~ minecraft:yellow_shulker_box
```

3\. ให้โคลนหรือก็อปปี้ช่องเก็บของของผู้เล่น ลงใน NBT บางอย่าง คุณสามารถโคลนมันไปไว้ที่ storage ได้เนื่องจากมันเร็วกว่า

```
data modify storage <storage> inventory set from entity <player> Inventory
```

4\. เนื่องจากจำนวนช่องที่จำกัดใน Shulker Box ทำให้มันไม่พอใช้กับจำนวนช่องเก็บของของผู้เล่น คุณต้องทำมันใน "batch" ซึ่งเป็นวิธีที่ง่ายที่สุดในการจัดการให้ตามลำดับทำได้โดยแยกมันออกมาเป็น "hotbar", "inventory", "armor" and "offhand" batch.
    <!-- - "hotbar" batch will contain items from slot 0 to 8 -->
    <!-- - "inventory" batch will contain items from slot 9 to 35 -->
    <!-- - "armor" batch will contain items from slot 100 to 103 -->
    <!-- - "offhand" btach will contain items from slot -106 (with the negative sign) -->

4.1. สร้าง batch บางอย่างด้วยคำสังประมาณนี้ ตัวอย่าง
```
                                      |--- Batch name                                     |---- Slot number
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 0b}]
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 1b}]
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 2b}]
.
.
.

// อย่างลืมลบ `Slot` nbt ออกจาก ไอเทม ก่อนที่จะไปทำขั้นตอนต่อไป!
data remove storage <storage> batch.hotbar[].Slot
```

4.2. แก้ไข NBT ของแต่ละไอเทมที่คุณต้องการ

5\. คัดลอก nbt จากขั้นตอนก่อนหน้าลงใน Shulker Box แล้วแทนที่ไอเทมเข้าไปยังช่องเก็บของของผู้เล่น

```
data modify block ~ ~ ~ Items set from storage <storage> batch.hotbar
loot replace entity <player> hotbar.0 9 mine ~ ~ ~ iron_pickaxe{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.inventory
loot replace entity <player> inventory.0 27 mine ~ ~ ~ iron_pickaxe{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.armor
loot replace entity <player> armor.feet 4 mine ~ ~ ~ iron_pickaxe{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.offhand
loot replace entity <player> weapon.offhand 1 mine ~ ~ ~ iron_pickaxe{drop_contents: 1b}
```

หมายเหตุ: อย่าลืมสังเกตดู Slot และก็ประเภทให้ตรงกันด้วยนะ

6\. เก็บกวาดหลังใช้งานเสร็จ

```
setblock ~ ~ ~ minecraft:air
data remove storage <storage> batch
data remove storage <storage> inventory
```

## หมายเหตุ

ในตัวอย่างเหล่านี้คิดว่าคุณต้องแก้ไข NBT ของ Slot ทั้งหมด แต่ถ้าคุณแก้ไขไอเทมช่อง Hotbar อย่างเดียว mainhand อย่างเดียว คุณไม่ต้องใช้ "inventory", "armor" and "offhand" batch.