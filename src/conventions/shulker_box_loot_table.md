# Shulker Box Loot Table แบบแผน

## คืออะไร

แบบแผนนี้มีวัตถุประสงค์เพื่อสร้างระบบ [จัดการช่องเก็บของของผู่เล่นด้วย Shulker Box](../tips/shulker_box_inventory_manipulation.md) เพื่อไม่ให้เกิดการชนกันภายในระบบ โดยการบังคับใช้ loot table สำหรับบล็อก `minecraft:yellow_shulker_box` โดยเฉพาะ

## โดยปฏิบัติดังต่อไปนี้

เพียงแค่สร้าง loot table ที่ `/data/minecraft/loot_tables/blocks/yellow_shulker_box.json` เมื่อคุณต้องการใช้เคล็ดลับการ [จัดการช่องเก็บของของผู้เล่น](../tips/shulker_box_inventory_manipulation.md) 

```json
{
  "type": "minecraft:block",
  "pools": [
    {
      "rolls": 1,
      "entries": [
        {
          "type": "minecraft:alternatives",
          "children": [
            {
              "type": "minecraft:dynamic",
              "name": "minecraft:contents",
              "conditions": [
                {
                  "condition": "minecraft:match_tool",
                  "predicate": {
                    "nbt": "{drop_contents: 1b}"
                  }
                }
              ]
            },
            {
              "type": "minecraft:item",
              "functions": [
                {
                  "function": "minecraft:copy_name",
                  "source": "block_entity"
                },
                {
                  "function": "minecraft:copy_nbt",
                  "source": "block_entity",
                  "ops": [
                    {
                      "source": "Lock",
                      "target": "BlockEntityTag.Lock",
                      "op": "replace"
                    },
                    {
                      "source": "LootTable",
                      "target": "BlockEntityTag.LootTable",
                      "op": "replace"
                    },
                    {
                      "source": "LootTableSeed",
                      "target": "BlockEntityTag.LootTableSeed",
                      "op": "replace"
                    }
                  ]
                },
                {
                  "function": "minecraft:set_contents",
                  "entries": [
                    {
                      "type": "minecraft:dynamic",
                      "name": "minecraft:contents"
                    }
                  ]
                }
              ],
              "name": "minecraft:yellow_shulker_box"
            }
          ]
        }
      ]
    }
  ]
}
```

## อ้างอิง

- [จัดการช่องเก็บของของผู่เล่นด้วย Shulker Box](../tips/shulker_box_inventory_manipulation.md)