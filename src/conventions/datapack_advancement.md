# Datapack Advancement Convention (แบบแผนแอดวานซ์เม้นท์ดาต้าแพค)

## คืออะไร

ข้อนี้มีวัตถุประสงค์เพื่อแจ้งข้อความการติดตั้งดาต้าแพค ซึ่งทำได้โดยการใช้แท็บ [Advancement](https://minecraft.gamepedia.com/Advancements) ในการแสดงผลแท็บการติดตั้งจาก root advancement แล้วโยงออกมาเป็นผู้สร้างดาต้าแพคแล้วต่อด้วยดาต้าแพคที่ผู้สร้างเขียน

แบบแผนนี้แบ่งออกเป็น 3 ส่วน: `Root`, `Namespace` และ `Datapack`

## 1. Root Advancement

นี่คือจุดเริ่มต้น Advancement ของทุกๆดาต้าแพคที่จะแยกสายออกมาจากข้อนี้ ไฟล์ Advancement นี้จะ **ต้อง** อยู่ที่ไฟล์ `/data/global/advancements/root.json` (สีพื้นหลังปัจจุบันที่ใช้คือ gray_concrete)

```json
{
    "display": {
        "title": "Installed Datapacks",
        "description": "",
        "icon": {
            "item": "minecraft:knowledge_book"
        },
        "background": "minecraft:textures/block/gray_concrete.png",
        "show_toast": false,
        "announce_to_chat": false
    },
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

## 2. Namespace Advancement

นี่คือ Advancement *หัวของ*ผู้สร้างแต่ละดาต้าแพค ซึ่งทุกๆของ*คุณ*ดาต้าแพคจะเหมือนกันหมด ซึ่งมัน **ควร** อยู่ที่ไฟล์ `/data/global/advancement/<namespace>.json`.

```json
{
    "display": {
        "title": "<Your name>",
        "description": "",
        "icon": {
            "item": "minecraft:player_head",
            "nbt": "{SkullOwner: '<your_minecraft_name>'}"
        },
        "show_toast": false,
        "announce_to_chat": false
    },
    "parent": "global:root",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

## 3. Datapack Advancement

นี่คือ Advancement สำหรับดาต้าแพคของ*คุณ*มันควรอยู่เดี่ยวๆจากดาต้าแพคอื่นๆ (แต่ไม่ได้บังคับเพราะบางทีคุณอาจจะต้องการบอกข้อมูลดาต้าแพคนั้นมากกว่า 1 อย่างเช่น EstMapUtility) คุณสามารถสร้างมันที่ไหนก็ได้ แต่ไม่ใช่ที่ โฟลเดอร์ `/data/global/advancements/` ซึ่งผมแนะนำว่า `/data/namespace/advancement/datapack_name/<datapack_name>.json`

```json
{
    "display": {
        "title": "<datapack name>",
        "description": "<datapack description>",
        "icon": {
            "item": "<item>"
        },
        "announce_to_chat": false,
        "show_toast": false
    },
    "parent": "global:<namespace>",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

### หมายเหตุ

ทุกอย่างที่อยู่ใน `<...>` ควรเปลี่ยนเป็นข้อมูลตามที่เขียนระบุไว้

## ผลลัพธ์

แท็บ Advancement ของคุณในตอนนี้มันควรจะเป็นแบบนี้ดังภาพ:

![Datapack Advancement Convention Preview](https://i.imgur.com/6bzBBr1.png)  
(Image by @Hashs#9531)
