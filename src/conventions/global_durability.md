# Global Durability (`*`)

## เกี่ยวกับ

แบบแผนนี้เป็นฟังก์ชั่นเสริมของ [Common Trait แบบแผน](./common_trait.md) โดยจัดเตรียมรูปแบบสำหรับ "ค่าความคงทนกำหนดเอง" ซึ่งมีประโยชน์สำหรับดาต้าแพ็คที่มีอุปกรณ์มีค่าความทนทาน

### การช้งาน

แบบแผนนี้จะมีอยู่ในแท็ก `ctc` โดยการทำตามแบบ [Common Trait แบบแผน](./common_trait.md)

```json
{
    ctc: {
        tool: {
            damage: 0,
            durability: 0,
            broken: false
        }
    }
}
```

ที่ไหน:

- `damage` คือค่าความเสียหายของอุปกรณ์ ซึ่งจะเหมือนกับฟังก์ชั่นแท็กจำนวน `Damage` ของตัวเกมต้นฉบับ
- `durability` คือค่าความทนทานสูงสุดของอุปกรณ์ หรือ ค่าความเสียหายของอุปกรณ์สามารถรับได้ หรือ จำนวนครั้งที่ใช้งานได้
- `broken` คือค่าข้อมูลจริงเท็จที่บอกว่าไอเทมนั้นพังหรือไม่ (เช่น [Broken Elytra](https://minecraft.gamepedia.com/Elytra)), แท็กนี้สามารถใช้ได้ในกรณีที่คุณไม่ต้องการให้ไอเทมถูกทำลายเมื่อค่าความทนทานหมดลง

## หมายเหตุ

แบบแผนนี้บังคับว่าแท็กควรอยู่ *ที่ไหน* เท่านั้น มันไม่ได้บังคับวิธีการลดค่าความทนทาน และในทำนองเดียวกันก็ไม่ได้บังคับว่าแท็กนี้ต้องสัมพันธ์กับแท็ก `damage` ต้นฉบับของเกม