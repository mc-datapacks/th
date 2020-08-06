# Common Trait แบบแผน

## คืออะไร

แบบแผนนี้มีวัตถุประสงค์เพื่อนำระบบ Ore Dictionary จาก [Forge](http://files.minecraftforge.net/) ไปยังไอเทมกำหนดเองของดาต้าแพค ทำได้โดยใช้คุณสมบัติ NBT ที่กำหนดเองของไอเทม เราสามารถใส่ NBT ภายในไอเทมใดก็ได้ที่เราสามารถใช้คำสั่งอื่นๆเพื่อเช็คหรือดูข้อมูลภายในของมันได้

โดยสิ่งนี้เราสามารถสร้างระบบการค้นหาแบบรวมที่ดาต้าแพคทุกอันสามารถใช้เพื่อหาไอเทมเฉพาะที่ต้องการ ซึ่งคุณสามารถใช้ syntax ที่ใส่ไว้ใน Common Trait Convention เพื่อสร้างฟังก์ชั่นเพื่อหาไอเทมที่คุณต้องการได้

### ตัวอย่างการใช้งาน

มันยากที่จะจินตนาการว่าแบบแผนนี้จะใช้ประโยชน์ในโลกจริงได้อย่างไร เราจึงรวบรวมการใช้งานที่มีประโยชน์ ซึ่งจะไม่สามารถทำได้หากไม่ทำตามแบบแผนที่ว่าเหล่านี้

1. สมมติว่าเราเพิ่ม `custom furnace` โดยคุณอยากจะหลอมแร่ทองแดงให้เป็นทองแดงแท่ง ด้วยแบบแผนนี้ คุณสามารถตรวจสอบแร่ทองแดง **ใดๆก็ได้** ก็ได้จากดาต้าแพคอื่นๆเพื่อหลอมมัน (สรุปก็คือ หากดาต้าแพคของคุณมีเตาเผาที่สร้างขึ้นมาเองหรือกำหนดเอง เมื่อทำตามแบบแผนนี้มันจะทำให้เตาเผาของคุณหลอมแร่ทองแดงจากทั่วทุกสารทิศที่เพิ่มเข้ามาจากดาต้าแพคของคนอื่นหรืออาจจะรวมไปถึงแร่ของม็อดด้วย)
2. สมมติว่าดาต้าแพคของคุณเพิ่ม `fridge ตู้เย็น` ที่รับเฉพาะไอเทมอาหารด้วยแบบแผนนี้คุณสามารถตรวจหาไอเทมอาหาร **ใดๆก็ได้** แม้แต่ไอเทมอาหารที่ถูกสร้างขึ้นมาเองจากดาต้าแพคอื่นๆได้
3. สมมติว่าดาต้าแพคของคุณได้เพิ่ม `custom anvil` ที่ทำให้คุณซ่อมของได้โดยตรงจากไอเทมที่ทดแทนกันได้แทนที่จะเป็นแร่เหล็ก เพชร ทอง เป็นต้น ด้วยแบบแผนนี้ คุณสามารถตรวจจับไอเทมวัสดุทดแทน **ใดๆก็ได้** จากดาต้าแพคอื่นๆแม้วัสดุของไอเทมจะไม่ตรงกัน (สรุปก็คือ ถ้าคุณจะซ่อมดาบเหล็กสักเล่ม คุณต้องใช้แร่เหล็ก แต่เมื่อมีแบบแผนนี้ `custom anvil` ของคุณก็สามารถใช้แร่ที่ทดแทนเหล็กได้ อลูมิเนียม ไทเทเนี่ยม และอื่นๆถ้าในเกมมี)

### Traits

Traits คือ อัตลักษณ์และคุณสมบัติที่ไอเทมสามารถมีได้ โดยการระบุคุณสมบัติพวกนี้ลงใน NBT ของไอเทม ดาต้าแพคอื่นๆจะสามารถอ้างอิงไอเทมนั้นๆผ่าน Traits แทนการใช้ ID ได้โดยตรง

Trait เป็น อาร์เรย์ของสตริงและมี trait แบบนี้ใน nbt (notice `traits: [...]`?)

```mcfunction
/give @s diamond{ctc: {traits: ["some", "trait", "here"], id: "example", from: "convention:wiki"}}
```

### Syntax

Syntax ของ Common Trait Convention จะเก็บอยู่ข้างใน `ctc` ของ nbt ไอเทม ภายใน`ctc` จะบรรจุข้อมูล: `id`, `from` และ `traits` nbts.ไว้

- `id`: ไอดีจะอยู่ภายในของไอเทมของคุณซึ่งมันไม่ควรใช้ภายนอกดาต้าแพคของคุณ และต้องไม่ซ้ำกันถายในดาต้าแพคที่คุณเขียน
- `from`: เป็นชื่อของคุณหรือ namespace ที่ระบุว่าไอเทมนั้นมาจากดาต้าแพคอะไร
- `traits`: เป็นอาร์เรย์ของ traits ที่คุณสามารถใช้เพื่ออ้างอิงไอเทมจากดาต้าแพคภายนอกได้

เราจะสมมติว่า syntax ดังต่อไปนี้เป็นโครงสร้าง NBT ของ คำสั่ง `/give`

```text
{
    ctc: {
        id: "my_copper_ore",
        from: "convention:wiki",
        traits: ["metal/copper", "block", "ore"]
    }
}
```

ลองดูที่ `traits` nbt

- `metal/copper`, trait traits นี้บอกเราว่าไอเทมนี้เป็นทองแดง
- `block`, trait นี้บอกเราว่าไอเทมนี้เป็นบล็อกที่สามารถวางได้
- `ore`, trait นี้บอกเราว่าไอเทมนี้เป็นแร่

#### เครื่องหมายสแลช

ในตัวอย่างจะเห็นว่ามีการใช้ `/` ใน `metal/copper`, ซึ่งจะใช้ก็ต่อเมื่อ trait (ลักษณะ)ของวัตถุไม่สามารถระบุได้เพียง 1 อย่าง เช่น ลักษณะพิเศษของ `orange` (ส้ม) หมายถึงอะไรได้บ้างมันเป็น *สี* ส้มหรือ *ส้ม* ที่เป็นผลไม้

ซึ่งเราจะใช้ เครื่องหมายสแลช เพื่อแยกประเภทเหล่านี้ให้ออก `color/orange` และ `fruit/orange`

## การนำไปใช้

ในการตรวจจับหรือเช็คคุณสมบัติบางอย่างคุณเพียงแค่ตรวจสอบ `traits` nbt ของไอเทม

> ตรวจจับว่าผู้เล่นถืออาวุธหรือไม่

```mcfunction
execute as @a if entity @s SelectedItem.tag.ctc{traits: ["tool/weapon"]} run ...
```

> ตรวจจับว่าภายในบล็อคเก็บของมีแร่ทองแดง

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc{traits: ["metal/copper", "ore"]} run ...
```

> ตรวจจับว่าภายในบล็อคเก็บของมีไอเทมที่วางได้

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc{traits: ["block"]} run ...
```

## trait พื้นฐาน

นี่คือ trait ของไอเทมที่คุณสามารถใช้ได้ แต่มันไม่ได้หมายความว่าคุณไม่สามารถสร้าง trait หรืออัตลักษณ์ใหม่สำหรับการใช้งานของคุณเอง แต่ถ้าคุณสมบัติของไอเทมที่เราระบุในตารางเหล่านี้เหมาะสมกับความต้องการของคุณ คุณก็ใช้มันแทนได้เลย

> ลิสต์ถูกแบ่งออกเป็นหลายกลุ่มและคุณ **ไม่ควร** ใช้ traits จากกลุ่มเดียวกันสองครั้ง

### กลุ่มของประเภทวัตถุ

trait แสดงเกี่ยวกับไอเทมที่ถืออยู่

|Trait|คำอธิบาย|
|-----|-----------|
|gas|สารที่เป็นก๊าช|
|liquid|ของเหลว|
|block|ไอเทมที่วางได้|
|item|ไอเทมปกติในไมน์คราฟต์|

### กลุ่มของประเภทพิเศษ

นี่คือ traits ทั่วไปของไมน์คราฟต์ม็อด ซึ่งจะช่วยให้ดาต้าแพคของคุณรวบรวมไอเทมที่ใช้กับดาต้าแพคของคุณทั้งหมดได้ด้วยแบบแผนนี้

> กลุ่มนี้เป็นข้อยกเว้นของกฎข้างต้นคุณสามารถใช้ traits หลายอย่างจากกลุ่มนี้ได้มากเท่าที่คุณต้องการ

|Trait|คำอธิบาย|
|-----|-----------|
|ore|บล็อกแร่ที่สามารถหาเจอได้จากถ้ำ|
|seed|ไอเทมที่ใช้ปลูกโตเป็นฟืชได้|
|flower|ไอเทมดอกไม้|
|grass|บล็อกที่สามารถแพร่กระจายจากบล็อกนึงไปยังอีกบล็อกนึงได้|
|sapling|บล็อกที่โตเป็นต้นไม้ได้|
|vegetable|ไอเทมอาหารจาก `seed`|
|log|ไอเทมที่ดรอปจากต้นไม้|
|planks|ไอเทมที่ได้จากการแปรรูปไม้ `log`|

### กลุ่มบีบอัด

traits นี้แสดงถึงไอเทมที่สามารถคราฟรวมกันให้บีบอัดอยู่ใน 1 ไอเทมได้ และ คราฟกลับออกมาแยกส่วนได้

ตัวอย่างเช่น:

- `redstone dust` -> `redstone block`
- `ice` -> `packed ice`
- `iron block` -> `iron ingot`

|Trait|คำอธิบาย|
|-----|-----------|
|packed|รูปแบบอัดแน่นที่สุดของไอเทม, ซึ่งปกติจะอยู่ในรูปของบล็อก|
|ingot|รูปแบบปกติของไอเทม, ซึ่งปกติจะอยู่ในรูปของแท่งแร่|
|nugget|รูปแบบไอเทมที่เล็กสุด, ซึ่งปกติจะอยู่ในรูปของนักเก็ต|

### กลุ่มที่กินได้

traits นี้แสดงถึงไอเทมที่ผู้เล่นสามารถกินได้ (รวมไปถึงเครื่องดื่ม)

|Trait|คำอธิบาย|
|-----|-----------|
|food|ไอเทมที่กินได้ทุกประเภท|

### กลุ่มชุดเกราะ

traits นี้แสดงถึงไอเทมที่ผู้เล่นหรือเอ็นทิตี้อื่นๆสามารถสวมใส่ได้

|Trait|คำอธิบาย|
|-----|-----------|
|armor|ไอเทมที่สวมใส่ได้ทุกประเภท|

### กลุ่มแยกย่อยของเครื่องมือ

> นี้ trait ใช้ [เครื่องหมายสแลช](#เครื่องหมายสแลช)!

traits นี้แสดงถึงไอเทม ที่สามารถใช้กับโลกได้

|Trait|คำอธิบาย|
|-----|-----------|
|tool/mining|ไอเทมนี้สามารถใช้ขุดบล็อกหรือบล็อกอื่นๆที่เกี่ยวข้อง|
|tool/chopping|ไอเทมนี้สามารถใช้ตัดไม้และวัสดุไม้|
|tool/tilling|ไอเทมนี้สามารถใช้ไถพรวนดิน|
|tool/watering|ไอเทมนี้สามารถใช้รดน้ำได้|
|tool/weapon|ไอเทมนี้สามารถใช้สู้กับมอนสเตอร์แลพผู้เล่นอื่น|

### กลุ่มย่อยของมณี

> นี้ trait ใช้ [เครื่องหมายสแลช](#เครื่องหมายสแลช)!

traits นี้แสดงถึงไอเทมที่มีโครงสร้างเป็นผลึก

|Trait|คำอธิบาย|
|-----|-----------|
|gem/diamond|อัญมณี เพชร|
|gem/ruby|อัญมณี ทับทิม|
|gem/emerald|อัญมณี มรกต|
|gem/sapphire|อัญมณี แซฟไฟร์ (ไพลิน)|
|gem/prismarine|พริสมารีน|
|gem/lapis|อัญมณี ลาพิส ลาซูลี|
|gem/obsidian|วัสดุ ออปซิเดี้ยน ใดๆ|
|gem/quartz|วัสดุ ควอตซ์ ใดๆ|
|gem/opal|อัญมณี โอปอล|

### กลุ่มย่อยของโลหะ

> นี้ trait ใช้ [เครื่องหมายสแลช](#เครื่องหมายสแลช)!

traits นี้แสดงถึงไอเทม ที่มักถูกเพิ่มโดยม็อด

|Trait|คำอธิบาย|
|-----|-----------|
|metal/iron|ไอเทมที่ทำจากเหล็ก|
|metal/gold|ไอเทมที่ทำจากทอง|
|metal/copper|ไอเทมที่ทำจากทองแดง|
|metal/aluminium|ไอเทมที่ทำจากอลูมิเนียม|
|metal/tin|ไอเทมที่ทำจากดีบุก|
|metal/silver|ไอเทมที่ทำจากเงิน|
|metal/lead|ไอเทมที่ทำจากตะกัว|
|metal/nickle|ไอเทมที่ทำจากนิคเกิล|
|metal/platinum|ไอเทมที่ทำจากทองคำขาว|

## อ้างอิง

- [Ore Dict](https://paleocrafter-mcforge.readthedocs.io/en/fix-mkdoc-update/utilities/oredictionary/)