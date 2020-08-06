# Custom Model ID ไอดีโมเดลกำหนดเอง (`***`)

## คืออะไร

ข้อนี้มีวัตถุประสงค์เพื่อลดการตีกันภายในระบบ [custom model data](https://minecraft.gamepedia.com/Model#Item_tags) ให้มากที่สุดโดยกำหนดรหัสเฉพาะสำหรับผู้สร้างดาต้าแพ็คทุกคนที่ใช้ Resource packs

## 1. ลงทะเบียนไอดีของคุณ

"id" คือเลขจำนวนเต็ม (Integer) ระหว่าง 1-999 ซึ่งเราจะใช้ namespaced custom model data เฉพาะเจาะจง เพื่อป้องกันการตีกันของ Resource packs  [resourcepacks](https://minecraft.gamepedia.com/Resource_pack).

คุณสามารถลงทะเบียนไอดีของคุณได้ที่นี่ [https://mc-datapacks.web.app](https://mc-datapacks.web.app/custom_model_id).

## 2. ใส่เลขด้านหน้าเลขโมเดลของคุณด้วยไอดีของคุณ

คุณต้องใส่เลขไอดีของคุณไว้ด้านหน้าเลขโมเดลของคุณในรูปแบบดังกล่าว ซึ่ง `XXX` เป็น ไอดีของคุณ และ `0000` เป็น custom model data เลขอะไรก็ได้ สูงสุดเต็มที่ 4 หลัก

|id|cmd|
|---|----|
|XXX|0000|

และนี่คือตัวอย่าง:

### 2.1. id = 42

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 420001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 420020}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 420300}, "model": "path/to/model/3"}
    ]
}
```

### 2.2. id = 808

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 8081001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 8082002}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 8083003}, "model": "path/to/model/3"}
    ]
}
```

### 2.3. id = 1

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 10001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 10010}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 10011}, "model": "path/to/model/3"}
    ]
}
```

## หมายเหตุ

แบบแผนนี้ไม่ได้บังคับใช้ ข้อจำกัดใดๆจากเลขที่มีจำนวน 8 หลักขึ้นไป นักสร้างดาต้าแพ็ค สามารถใช้อันนี้เพื่อเพิ่มช่องจำนวนโมเดลเพิ่มเติมได้หากต้องการ (จำนวนหน่วยจะนับจากขวาไปซ่ายแน่นอน)