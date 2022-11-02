# AutoCrafter

This is a minecraft datapack which creates a block, that automatically crafts certain items.  
The item that should be crafted can be inserted in a simple GUI. the ingredients for the item can be put into the autocrafter block using a hopper. The output item can   be taken out of the autocrafter using a hopper or a hopper minecart

At the moment it isn't possible to craft every item. More items will be added somewhen. It is also possible to add <a href='https://github.com/shueppin/AutoCrafter-Datapack#how-to-add-your-own-recipes'>your own crafting recipes</a>.  
Go <a href='AutoCrafter/craftable_items.txt'>here</a> for a list of all craftable items.



# Installation

Click on `Code` and then `Download ZIP`. Open this ZIP file and put the directory `AutoCrafter` into the datapacks folder of your minecraft world. 
<a href='https://minecraft.fandom.com/wiki/Tutorials/Installing_a_data_pack'>Tutorial for installing a datapack</a>



# How to get the autocrafter

Put a `crafting table`, a `dropper` and a `chest` into the crafting slots of your inventory or a crafting table.  
As a result you should get a green book (called knowledge book). Then this will automatically be replaced with a Spawn egg for the AutoCrafter.  
Place this spawn egg, where you want your AutoCrafter to be. Note: the autocrafter is 2 blocks tall and 1 by 1 block wide.

You can configure the AutoCrafter by sneaking and rightclicking on the chest of the AutoCrafter.  
Then an inventory will open, in which you can put the item, it should craft into the top slot.  
It will then tell you, if that item is craftable or not (Open the file `AutoCrafter/craftable_items` in your datapack or go <a href='https://github.com/shueppin/AutoCrafter-Datapack/blob/master/autocrafter/craftable_items.txt'>here</a> to see a list of all craftable items.)

The input materials can then be put into the dropper (the top block of the AutoCrafter) using hoppers or by hand.  
The output can then be collected from the chest (the bottom block of the AutoCrafter) using a hopper or a hopper minecart, but not by hand.  
*Note:* The AutoCrafter will only craft items, as long as the chest is empty. This means, the chest has to be emptied (as mentioned above) all the time or the AutoCrafter won't work.



# How to add your own recipes

At first download the datapack.

<br/>

**Creation of the recipe file**

1. Go to `AutoCrafter/data/autocrafter/functions/crafting/templates`.  
2. Count the number of different input materials (items) your recipe needs.  
3. Copy the needed template. (For example if you have 2 different materials, then use the file `template_for_2_materials.mcfunction`)

It should look someting like this (this is the template of the recipe wihich uses only one material):

```mcfunction
scoreboard players set @s material_1_count_needed input_count_#
data modify storage autocrafter:crafting material_1_id set value "minecraft:input_item_#"


data modify storage autocrafter:crafting output_material set value {id:"minecraft:output_item", Count:output_count}


function autocrafter:crafting/functions/material_1/craft_item_with_1_material
```

4. Replace the following things: 

- Replace all `minecraft:input_item_#` (where `#` is the index of the material) with the exact name of the item.  
  Attention: The `minecraft:` in front of the item name is very important!

- Replace all `input_count_#` (where `#` is the index of the material) with the count of the needed material. 

- Replace the `minecraft:output_item` with the exact name of the item it should craft.  
  Attention: The `minecraft:` in front of the item name is very important!

- Replace the `output_count` with the number of items it should craft.  
  Attention: It is very important to add a `b` after the number.

At the end it should look something like this (Here it is the recipe of the goldblock, therefore it uses only one material.):

```mcfunction
scoreboard players set @s material_1_count_needed 9
data modify storage autocrafter:crafting material_1_id set value "minecraft:gold_ingot"


data modify storage autocrafter:crafting output_material set value {id:"minecraft:gold_block", Count:1b}


function autocrafter:crafting/functions/material_1/craft_item_with_1_material
```

*Note for recipes using items from mods*  
Instead of the `minecraft:` in front of the item names the name of the mod should be added instead: `modname:input_item_#`

<br/>

5. Save this file in the folder `AutoCrafter/data/autocrafter/functions/crafting/recipes`. It is recommended to use the name of the output item as filename. The suffix of the file is `.mcfunction`.

<br/>
<br/>

**Adding the file to the AutoCrafter**

1. Open the file `AutoCrafter/data/autocrafter/functions/get_item_to_craft.mcfunction`.
2. Copy the following line into the opened file. It is recommended to put the line into the area marked with `/----------\` and `\----------/`. In the file is also a comment to see where to put the line (the comment is marked with an arrow).

```mcfunction
execute if entity @s[nbt={Items:[{Slot:2b, id:"minecraft:output_item"}]}] store success score @s can_craft_item run function autocrafter:crafting/recipes/filename
```

3. Replace in the pasted line `minecraft:output_item` with the exact name of the output item of the created recipe.  
   Attention: The `minecraft:` in front of the item name is very important!

4. Replace in the pasted line `filename` with the filename of the created recipe. This should be the output item name.  
   Attention: Do *not* put the suffix `.mcfunction` there.

At the end it should look something like this:

```mcfunction
execute if entity @s[nbt={Items:[{Slot:2b, id:"minecraft:gold_block"}]}] store success score @s can_craft_item run function autocrafter:crafting/recipes/gold_block
```
