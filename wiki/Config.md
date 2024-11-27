## Ji Config

The [Config](https://github.com/drkhodakarami/JiConfig) library. This library contains 4 class, one enum and one interfaces. Your main entry to the whole system will be the abstract `Config` class.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiConfig) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/config/) repository for the project.

## ConfigKeyCasing

This enum is used internally to set the letter casing for the key section in your config file. If you use the `load(ConfigKeyCasing casing)` method on `Config` class, you can set your own letter casing instead of the default one.

```java
\\original key "Some.Key.String", casing "ALL_UPPER_CASE", result:
SOME.KEY.STRING=....
\\original key "Some.Key.String", casing "ALL_LOWER_CASE", result:
some.key.string=....
\\original key "SomeKeyString", casing "NO_CHANGE", result:
SomeKeyString=....
```

## IConfigProvider

This interface is used internally by the system.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to implement or extend this interface. This interface is for config system's internal use only.
</div>

## ConfigRequest class

This class is the request for a config object. It holds different important values like the file, the file name, the provider that is instance of `IConfigProvider`, the final `BaseConfig` and the final config string.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod or call any methods from this class and store the return values. This class is for config system's internal use only.
</div>

---
---
> ##### ***`ConfigRequest(File file, String filename)`***

The constructor of the class.

---
---
> ##### ***`getFile()`***

This method returns the `File` associated to the config system.

---
---
> ##### ***`getFilename()`***

This method returns the `Filename` associated to the config system.

---
---
> ##### ***`provider(IConfigProvider provider)`***

This will return an instance of `ConfigRequest` that is being instantiated in the system before hand.

---
---
> ##### ***`request(String modId, ConfigKeyCasing casing)`***

This method will return an instance of `BaseConfig` that is being instantiated before hand with the proper settings for the Mod ID and the key letter casing. Normally you don't need to call this method and save the return value because this method is being used internally by the system.

---
---
> ##### ***`getConfig()`***

This method will return a string containing all the config comments, key/pair values being set in it before hand.

## Registers class

This is the main helper method in the library. Most of the registry calls for Minecraft need the Mod ID of your mod. This class is a helper one and shouldn't be instantiated. Because of this, it has an internal modID attribute and you need to call the init(String modID) from the start of your main mod's initialization class (alternatively, if you are concerned about race condition between different mods calling the class, you can initialize the Registers at the start of each registry call in different classes, more about this on another page):

```java
public class Main implements ModInitializer
{
    public static String ModID = "your_mod_id";
  
    @Override
    public void onInitialize()
    {
        Registers.init(ModID);
    }
}
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, every registring of Items, Blocks, Block Entities, or any other entry into Minecraft's registry system, should happen after this call for the init method on Registers. If you mess things up for the order of operation, the default value for the Mod ID will be `default` and not only you will get warnings or errors about missing stuff in the consol, also the models and textures will not work properly any more.
</div>

The registers class has many sub classes, each handling dedicated section of registry entry.

---
---
> ##### ***`getKey(String name, RegistryKey<? extends Registry<T>> registryKey)`***

This method will return a RegistryKey for the given type. This value is needed for registering anything in the Minecraft registry system (from 1.21.3).

You can get any registry key like this:

```java
RegistryKey<Block> key = getKey(name, RegistryKeys.BLOCK);
```

Just change the RegistryKeys.BLOCK to anything you need from the RegistryKeys class.


### Block Sub Class

This sub class handles registering every `block` into the Minecraft's registry system.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModBlocks class and you should register the blocks in that class. In the ModBlocks class you can add `import static jiraiyah.register.Registers.Block.*` and by adding this import, you can drop the `Registers.Block.` section from all the examples provided for this sub class.

---
---
> ##### ***`registerSimple(String name, T block)`***

This is the old fasion way of registering a block. To use this method, you should provide an instance of a block that will cover the creation of the settings. 

Example usage:
```java
Block SOME_BLOCK = Registers.Block.registerSimple("some_block", new Block(AbstractBlock.Settings.creat().registryKey(someKey));
CustomBlock SOME_OTHER_BLOCK = Registers.Block.registerSimple("some_other_block", new CustomBlock(AbstractBlock.Settings.copy(Blocks.STONE).registryKey(someKey));
```

---
---
> ##### ***`register(String name)`***

When you want to register a decorative block that has nothing other than the default settings as a block, what you need to do is just register an instance of a vanilla block with default settings. This method will register your block as such a block. 

Example usage:
```java
Block SOME_BLOCK = Registers.Block.register("some_block");
```

---
---
> ##### ***`register(String name, List<Text> tooltips)`***

If you want a simple block with default settings that will show custom tooltips, you can use this method. This method will register a block as a simple block with default settings but adds the provided tooltips to the block.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Block SOME_BLOCK = Registers.Block.register("some_block", tooltips);
```

---
---
> ##### ***`register(String name, Block blockCopy)`***

If you want to register a simple block that copies it's settings from another block, you can use this method. The settings for the default block class will be copied from blockCopy parameter. 

Example usage:
```java
Block SOME_BLOCK = Registers.Block.register("some_block", Blocks.OBSIDIAN);
```

---
---
> ##### ***`register(String name, Block blockCopy, List<Text> tooltips)`***

If you want to register a simple block that copies it's settings from another block and add custom tooltips to this block, you can use this method call.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Block SOME_BLOCK = Registers.Block.register("some_block", Blocks.OBSIDIAN, tooltips);
```

---
---
> ##### ***`register(String name, AbstractBlock.Settings settings)`***

You can use this method to register a simple block with provided custom settings. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
Block SOME_BLOCK = Registers.Block.register("some_block", AbstractBlock.Settings.create());
```

---
---
> ##### ***`register(String name, AbstractBlock.Settings settings, List<Text> tooltips)`***

If you want to register a simple instance of block class with custom settings and tooltips, you can use this method call. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Block SOME_BLOCK = Registers.Block.register("some_block", AbstractBlock.Settings.create(), tooltips);
```

---
---
> ##### ***`register(String name, Function<AbstractBlock.Settings, T> factory)`***

When you want to register an instance of a custom block class you can use the provided method with factory as parameter. This method will register the custom block with default settings.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
---
> ##### ***`register(String name, Block blockCopy, Function<AbstractBlock.Settings, T> factory)`***

When you want to register an instance of a custom block class with copying the settings of another block, you can use the method with factory as parameter.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", Blocks.OBSIDIAN, CustomBlock::new);
```

---
---
> ##### ***`register(String name, AbstractBlock.Settings settings, Function<AbstractBlock.Settings, T> factory)`***

When you want to register an instance of a custom block class with custom properties, you can use the method with factory as parameter. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", AbstractBlock.Settings.create(), CustomBlock::new);
```

---
---
> ##### ***`registerStair(String name, Block stateBlock, Block copyBlock)`***

This is a helper method to register `stair` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerStair("some_stair", ModBlocks.RUBY, Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerSlab(String name, Block copyBlock)`***

This is a helper method to register `slab` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerSlab("some_slab", Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerButton(String name, BlockSetType blockType, int pressureTicks, Block copyBlock)`***

This is a helper method to register `button` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerButton("some_button", BlockSetType.IRON, 40, Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerPressurePlate(String name, BlockSetType blockType, Block copyBlock)`***

This is a helper method to register `pressure plate` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerPressurePlate("some_plate", BlockSetType.IRON, Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerFence(String name, Block copyBlock)`***

This is a helper method to register `fence` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerFence("some_fence", Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`FenceGateBlock registerFenceGate(String name, WoodType woodType, Block copyBlock)`***

This is a helper method to register `fence gate` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerFenceGate("some_fence_gate", WoodType.OAK, Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerWall(String name, Block copyBlock)`***

This is a helper method to register `wall` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerWall("some_wall", Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerDoor(String name, BlockSetType blockType, Block copyBlock)`***

This is a helper method to register `door` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerDoor("some_door", BlockSetType.IRON, Blocks.AMETHYST_BLOCK);
```

---
---
> ##### ***`registerTrapdoor(String name, BlockSetType blockType, Block copyBlock)`***

This is a helper method to register `trap door` blocks. This method will call the registerSimple internally.

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.registerTrapdoor("some_trap_door", BlockSetType.IRON, Blocks.AMETHYST_BLOCK);
```

### Entities Sub Class

This sub class handles registering every `entity` (including block entity) into the Minecraft's registry system.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModEntities class and you should register the block entities or any other entity in that class. In the ModEntities class you can add `import static jiraiyah.register.Registers.Entities.*` and by adding this import, you can drop the `Registers.Entities.` section from all the examples provided for this sub class.

---
---
> ##### ***`register(String name, Block block, FabricBlockEntityTypeBuilder.Factory<T> factory)`***

When you want to register a `block entity` that is associated with a block, you can call the register method and provide the block entity class as the factory.

Example usage:
```java
CustomBlockEntity SOME_BLOCK_ENTITY = Registers.Entities.register("some_block_entity", ModBlocks.SOME_BLOCK, SomeBlockEntity::new);
```

---
---
> ##### ***`register(String name, EntityType.EntityFactory<T> factory)`***

When you want to register an entity, you can use this method to register the entity.

Example usage:
```java
CustomEntity SOME_ENTITY = Registers.Entities.register("some_entity", CustomEntity::new);
```

### Item Sub Class

This sub class handles registering every `item` into the Minecraft's registry system. It has methods for registering tools, armor, food, and snack food as helper methods.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModItems class and you should register the items in that class. In the ModItems class you can add `import static jiraiyah.register.Registers.Item.*` and by adding this import, you can drop the `Registers.Item.` section from all the examples provided for this sub class.

---
---
> ##### ***`register(String name)`***

If you want to simply register a simple item with default settings, you can use this method. The method will register an instance of item class with given name and default settings.

Example usage:
```java
Item SOME_ITEM = Registers.Item.register("some_item");
```

---
---
> ##### ***`register(String name, List<Text> tooltips)`***

If you want to register a simple item with default settings but with the addition of tooltips, you can use this method. The method will register an instance of item class with given name and tooltops and default settings.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_ITEM = Registers.Item.register("some_item", tooltips);
```

---
---
> ##### ***`register(String name, int stackCount)`***

If you want to define the stack size for a simple item with default settings, you can use this method. The method will register an instance of item class with given stack size being added on top of default settings.

Example usage:
```java
Item SOME_ITEM = Registers.Item.register("some_item", 1);
```

---
---
> ##### ***`register(String name, int stackCount, List<Text> tooltips)`***

If you want to define the stack size and tooltips for a simple item with default settings, you can use this method. The method will register an instance of item class with given stack size being added on top of default settings with addition of tooltips being set for the instance.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_ITEM = Registers.Item.register("some_item", 1, tooltips);
```

---
---
> ##### ***`register(String name, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class, you can use this method that will accept the custom class as factory.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", CustomItem::new);
```

---
---
> ##### ***`register(String name, int stackCount, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with definition of stack size, you can use this method call and send the stack size and the item class factory in.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", 1, CustomItem::new);
```

---
---
> ##### ***`register(String name, int stackCount, Item.Settings settings, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with definition of stack size and your own custom settings, you can call this method. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", 1, new Item.Settings(), CustomItem::new);
```

---
---
> ##### ***`register(String name, Item.Settings settings, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with custom settings and default stack size, you can use this method call and provide the settings and the custom class as factory. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", new Item.Settings(), CustomItem::new);
```

---
---
> ##### ***`registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, IToolFactory<Item.Settings, T> factory)`***

This method can be used to register any tool item. You will send in the material and other parameters of the tool item and the tool class as the factory.

Example usage:
```java
AxeItem SOME_AXE = Registers.Item.registerTool("some_axe", ModToolMaterials.MATRERIAL, 0.5f, 0.5f, AxeItem::new);
HoeItem SOME_HOE = Registers.Item.registerTool("some_hoe", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, HoeItem::new);
PickaxeItem SOME_PICKAXE = Registers.Item.registerTool("some_pickaxe", ModToolMaterials.MATRERIAL, 0.1f, 0.1f, PickaxeItem::new);
ShovelItem SOME_SHOVEL = Registers.Item.registerTool("some_shovel", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, ShovelItem::new);
SwordItem SOME_SWORD = Registers.Item.registerTool("some_sword", ModToolMaterials.MATRERIAL, 1.5f, -2.0f, SwordItem::new);
CustomTool SOME_TOOL = Registers.Item.registerTool("some_tool", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, CustomTool::new);
```

---
---
> ##### ***`registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, Item.Settings settings, IToolFactory<Item.Settings, T> factory)`***

If you want to register a tool but instead of using the default settings, you want to set your custom settings, you can use this method. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
AxeItem SOME_Axe = Registers.Item.registerTool("some_tool", ModToolMaterials.MATERIAL, 0.0f, 0.0f, new Item.Settings(), AxeItem::new);
```

---
---
> ##### ***`registerArmor(String name, ArmorMaterial material, EquipmentType equipment, IArmorFactory<Item.Settings, T> factory)`***

This method will register an armor item with default settings. You can simply send `ArmorItem` class as the factory or alternatively, you can send any custom class that is extending the armor item one.

Example usage:
```java
ArmorItem SOME_HELMET = Registers.Item.registerArmor("some_helmet", ModArmorMaterials.MATERIAL, EquipmentType.HELMET, ArmorItem::new);
ArmorItem SOME_CHESTPLATE = Registers.Item.registerArmor("some_chestplate", ModArmorMaterials.MATERIAL, EquipmentType.CHESTPLATE, ArmorItem::new);
ArmorItem SOME_LEGGINGS = Registers.Item.registerArmor("some_leggings", ModArmorMaterials.MATERIAL, EquipmentType.LEGGINGS, ArmorItem::new);
ArmorItem SOME_BOOTS = Registers.Item.registerArmor("some_boots", ModArmorMaterials.MATERIAL, EquipmentType.BOOTS, ArmorItem::new);
```

---
---
> ##### ***`registerArmor(String name, ArmorMaterial material, EquipmentType equipment, Item.Settings settings, IArmorFactory<Item.Settings, T> factory)`***

If you want to register an armor item with your custom settings, you can use this method call. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
ArmorItem SOME_BOOTS = Registers.Item.registerArmor("some_boots", ModArmorMaterials.MATERIAL, new Item.Settings(), EquipmentType.BOOTS, ArmorItem::new);
```

---
---
> ##### ***`registerSnackFood(String name, int stackCount, int nutrition, float saturation)`***

This method will register an item as a food with the `alwaysEdible` flag on the settings that will make the food as a snack one.

Example usage:
```java
Item SOME_SNACK = Registers.Item.registerSnackFood("some_snack", 16, 4, 0.55f);
```

---
---
> ##### ***`registerSnackFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`***

This method will register an item as a food with the `alwaysEdible` flag on the settings that will make the food as a snack one. In addition, it will add the provided tootips to the item.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_SNACK = Registers.Item.registerSnackFood("some_snack", 16, 4, 0.55f, tooltips);
```

---
> ##### ***`registerFood(String name, int stackCount, int nutrition, float saturation)`***

This method will register an item as a food but it will not add `alwaysEdible` flag on the settings. In other words, the food item that is registered using this method is not a snack.

Example usage:
```java
Item SOME_FOOD = Registers.Item.registerFood("some_food", 16, 4, 0.55f);
```

---
---
> ##### ***`registerFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`***

This method will register an item as a food but it will not add `alwaysEdible` flag on the settings. In other words, the food item that is registered using this method is not a snack. In addition, it will add the provided tootips to the item.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_FOOD = Registers.Item.registerFood("some_food", 16, 4, 0.55f, tooltips);
```

### BlockItem Sub Class

This sub class handles registering every `block item` into the Minecraft's registry system. keep in mind that the methods provided in the item sub class are not suitable for registering any block item. From Minecraft version 1.21.3 and forward, block items need to have a flag set on the settings. The methods on the item sub class will not set this flag for any of the registered items. If you want to register a block item, you need to use methods in the block item sub class.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModBlockItems class and you should register the block items in that class. In the ModBlockItems class you can add `import static jiraiyah.register.Registers.BlockItem.*` and by adding this import, you can drop the `Registers.BlockItem.` section from all the examples provided for this sub class.

> ##### ***`register(Block block)`***

This will register a simple block item for the given block. The block item will have the default settings.

Example usage:
```java
BlockItem SOME_BLOCK_ITEM = Registers.BlockItem.register(ModBlocks.SOME_BLOCK);
```

---
---
> ##### ***`register(Block block, IBlockItemFactory<Item.Settings, T> factory)`***

This method will register a custom block item type for the given block. Using this method, you should provide the custom block item class (that extends block item class) as the factory. The registerd block item will have default settings.

Example usage:
```java
CustomBlockItem SOME_BLOCK_ITEM = Registers.BlockItem.register(ModBlocks.SOME_BLOCK, CustomBlockItem::new);
```

---
---
> ##### ***`register(Block block, Item.Settings settings, IBlockItemFactory<Item.Settings, T> factory)`***

This method will register a custom block item type with custom settings for the given block. Take note that in the settings section we are not adding `.registryKey(someKey)` or `.useBlockPrefixedTranslationKey()` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomBlockItem SOME_BLOCK_Item = Registers.BlockItem.register(ModBLocks.SOME_BLOCK, new Item.Settings(), CustomBlockItem::new);
```

### Recipe Sub Class

This sub class handles registering every custom `recipe` into the Minecraft's registry system.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModRecipes class and you should register the recipes in that class. In the ModRecipes class you can add `import static jiraiyah.register.Registers.Recipe.*` and by adding this import, you can drop the `Registers.Recipe.` section from all the examples provided for this sub class.

> ##### ***`register(String name, RecipeSerializer<?> serializer)`***

This method will register a given recipe serializer into the registry system using the given name.

Example usage:
```java
RecipeSerializer<?> RECIPE_SERIALIZER = Registers.Recipe.register("some_recipe_serializer", ModRecipes.SOME_SERIALIZER);
```

---
---
> ##### ***`register(String name, RecipeType<?> recipeType)`***

This method will register a given recipe type into the registry system using the given name.

Example usage:
```java
RecipeType<?> RECIPE_TYPE = Registers.Recipe.register("some_recipe_type", ModRecipes.SOME_TYPE);
```

### ComponentType Sub Class

This sub class handles registering every custom `data component` type into the Minecraft's registry system.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModComponents class and you should register the custom data components in that class. In the ModComponents class you can add `import static jiraiyah.register.Registers.ComponentType.*` and by adding this import, you can drop the `Registers.ComponentType.` section from all the examples provided for this sub class.

> ##### ***`register(String name, UnaryOperator<ComponentType.Builder<T>> buildOperator)`***

This will register a custom data component type in the Minecraft registry system. Keep in mind, as standard, any custom data type should have a codec definition in itself and in the provided example, the `CustomData.CODEC` is that static codec definition field in the custom data class or record. Take note, this is the only method in the Registers class and it's sub classes that accepts a lambda builder.

Example usage:
```java
ComponentType<CustomData> SOME_DATA_TYPE = Registers.ComponentType.register("some_data_type", builder -> builder.codec(CustomData.CODEC));
```

### StatusEffect Sub Class

This sub class handles registering every custom `status effect` into the Minecraft's registry system.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have a ModEffects class and you should register the status effect in that class. In the ModEffects class you can add `import static jiraiyah.register.Registers.StatusEffect.*` and by adding this import, you can drop the `Registers.StatusEffect.` section from all the examples provided for this sub class.

> ##### ***`register(String name, StatusEffectCategory category, int color, BiFunction<StatusEffectCategory, Integer, StatusEffect> factory)`***

This method will register a custom status effect into Minecraft's registry system. Keep in mind, the color provided here will be used for coloring the particle effects. Also, the category is a value from `StatusEffectCategory` enums.

Example usage:
```java
RegistryEntry<StatusEffect> SOME_EFFECT = Registers.StatusEffect.register("some_effect", StatusEffectCategory.BENEFICIAL, 0xFFFFFF, CustomEffect::new);
```

### Datagen Sub Class

This sub class does not register anything into the Minecraft's registry system however, it's responsible of adding helper methods related to `datagen`.

In the use cases provided here, we are addressing the methods with complete class address. However, normally you will have different classes handling datagen in themselves. In each of these classes you can add `import static jiraiyah.register.Registers.Datagen.*` and by adding this import, you can drop the `Registers.Datagen.` section from all the examples provided for this sub class.

---
---
> ##### ***`registerAllArmor(ItemModelGenerator generator, Item[] items, ArmorMaterial material)`***

This method is related to ItemModelProvider for datagen. It's responsible of providing the item model for the armor items. You can directly call this method from item model provider and give an array of items that are armors sharing the same ArmorMaterial.

Example usage:
```java
Registers.Datagen.registerAllArmor(itemModelGenerator, new Item[]{ModItems.HELMET, 
                                                                  ModItems.CHESTPLATE, 
                                                                  ModItems.LEGGINGS, 
                                                                  ModItems.BOOTS},
                                   ModArmorMaterials.MATERIAL);
```

---
---
> ##### ***`registerArmor(ItemModelGenerator generator, Item item, ArmorMaterial material, EquipmentSlot slot)`***

This method is related to ItemModelProvider for datagen. It's responsible of providing the item model for a single armor item. You can directly call this method from item model provider and generate the sinle armor item json files.

Example usage:
```java
Register.Datagen.registerArmor(itemModelGenerator, ModItems.HELMET, ModArmorMaterial.MATERIAL, EquipmentSlot.HEAD);
```

---
---
> ##### ***`customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item, float min, float max)`***

This method is related to the LootTableProvider for datagen. It's responsible of providing the information needed to creating the loot drop for custom ore when an ore block is broken. You can directly call this method and give it as a param for the addDrop method. Normally you should call this inside the `generate` method of a class that extends `FabricBlockLootTableProvider`. Take note, `this` in the method call example, is refering to the extended class that you are writing the code in it's generate method.

Example usage:
```java
public class ModLootTableProvider extends FabricBlockLootTableProvider
{
    public void generate()
    {
        addDrop(ModBlocks.SOME_ORE, Registers.Datagen.customOreDrops(this, this.registries,
                                                                     ModBlocks.SOME_ORE, ModItems.SOME_ORE_DROP,
                                                                     2.0f, 3.0f);
    }
}
```

---
---
> ##### ***`customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item)`***

This is an overload for the customOreDrop method we explained above. This overload, will use minimum of 2 and maximum of 5 drops by default. The rest of the information is the same as above.

Example usage:
```java
public class ModLootTableProvider extends FabricBlockLootTableProvider
{
    public void generate()
    {
        addDrop(ModBlocks.SOME_ORE, Registers.Datagen.customOreDrops(this, this.registries,
                                                                     ModBlocks.SOME_ORE, ModItems.SOME_ORE_DROP);
    }
}
```

---
---
> ##### ***`registerOrientableVariantBlock(BlockStateModelGenerator generator, Block block, BooleanProperty property)`***

This method is related to the BlockModelProvider for datagen. It's responsible of registering a model for a block that has variants for a boolean property. This will create the json file in such a way that it will accept the boolean property as the variant changing factor. Remember, you need to have two textures in the resources section to handle two variants for the front face. The model is generated using `TexturedModel.ORIENTABLE` meaning that you should provide front, side, and top textures for the block. You can directly call this method from block model provider to create the json file. Take not `ACTIVATED` in the example bellow, is the boolean property sitting inside the `SomeBlock` class. In this example, the front texture should have two textures in the resource folder, one as `some_block_front` and one as `some_block_front_activated`.

Example usage:
```java
Registers.Datagen.registerOrientableVariantBlock(generator, ModBlocks.SOME_BLOCK, SomeBlock.ACTIVATED);
```

---
---
> ##### ***`registerCubeVariantBlock(BlockStateModelGenerator generator, Block block, BooleanProperty property)`***

This method is an overload for the above one. The difference is that, instead of using `TextureModel.ORIENTABLE` it uses `TexturedModel.CUBE_ALL`. Because of this, you need to provide only two textures for the block, one texture for the normal state of the block and one for the variant with the property. In this example, `ACTIVATED` is the boolean property sitting inside the block class. Because of this, you have to provide two textures in the resource section, one as `some_block` and one as `some_block_activated`.

Example usage:
```java
Registers.Datagen.registerCubeVariantBlock(generator, ModBlocks.SOME_BLOCK, SomeBlock.ACTIVATED);
```

---
---
> ##### ***`buildHumanoid(String name)`***

This method is related to ItemModelProvider for armor datagen. Normally you don't need to call this method directly because when you use the provided `registerArmor` or `registerAllArmor` methods, they internally use this method. This method is responsible of generating the humanoid models for the armor item and return `EquipmentModel` to be used for the model provider.

Example usage:
```java
EquipmentModel humanoid_model = Registers.Datagen.buildHumanoid("emerald");
```

---
---
> ##### ***`buildHumanoidAndHorse(String name)`***

This method is related to ItemModelProvider for armor datagen. Normally you don't need to call this method directly because when you use the provided `registerArmor` or `registerAllArmor` methods, they internally use this method. This method is responsible of generating the humanoid and horse models for the armor item and return `EquipmentModel` to be used for the model provider.

Example usage:
```java
EquipmentModel humanoid_model = Registers.Datagen.buildHumanoidAndJorse("emerald");
```

---
---
> ##### ***`register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, List<PlacementModifier> modifiers)`***

This method is related to ore generation and datagen related to spawning ore in the world. The method will register a `placed feature` inside the registry system. You can directly call this method from a bootstrap method to register a placed feature.

Example usage:
```java
public class ModPlacedFeatures
{
    public static void bootstrap(Registerable<PlacedFeature> context)
    {
        List<PlacementModifier> modifiers = ...;\\ Here you create the list of modifiers
        Registers.Datagen.register(context, SOME_ORE_PLACED_KEY,
                           lookup.getOrThrow(ModConfiguredFeatures.SOME_FEATURE),
                           modifiers);
    }
}
```

---
---
> ##### ***`register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, PlacementModifier... modifiers)`***

This method is an overload for the previous one. Instead of accepting a list of modifiers, you should provide modifiers one after another with a `,` in between them. In the example bellow, we add a single modifier that changes the height limit of ore generation.

Example usage:
```java
public class ModPlacedFeatures
{
    public static void bootstrap(Registerable<PlacedFeature> context)
    {
        List<PlacementModifier> modifiers = ...;\\ Here you create the list of modifiers
        Registers.Datagen.register(context, SOME_ORE_PLACED_KEY,
                           lookup.getOrThrow(ModConfiguredFeatures.SOME_FEATURE),
                           Registers.Datagen.modifiersWithCount(1, HeightRangePlacementModifier.uniform(YOffset.fixed(50), 
                                                                                                        YOffset.fixed(60))));
    }
}
```

---
---
> ##### ***`register(Registerable<ConfiguredFeature<?, ?>> context, RegistryKey<ConfiguredFeature<?, ?>> key, F feature, FC configuration)`***

This method is related to ore generation and datagen related to spawning ore in the world. The method will register a `configured feature` inside the registry system. You can directly call this method from a bootstrap method to register a placed feature.

Example usage:
```java
public class ModConfiguredFeatures
{
    public static void bootstrap(Registerable<ConfiguredFeature<?, ?>> context)
    {
        RuleTest ruleTest = ...;// You will create the proper rule test for ore generation here.
        Registers.Datagen.register(context, SOME_ORE_KEY, Feature.ORE,
                           new OreFeatureConfig(ruleTest, 8));//8 here is the vein size
    }
}
```

---
---
> ##### ***`modifiers(PlacementModifier countModifier, PlacementModifier heightModifier)`***

The method is related to world generation and datagen related to spawning in the world. This method will return a list of `PlacementModifier`. It accepts a count of placement, and height limiting factors. Normally you will call this method from within a placed feature registration.

Example usage:
```java
List<PlacementModifier> modifiers = Registers.Datagen.modifiers(CountPlacementModifier.of(5),
                                                                HeightRangePlacementModifier.uniform(YOffset.fixed(50), 
                                                                                                     YOffset.fixed(60)));
```

---
---
> ##### ***`modifiersWithCount(int count, PlacementModifier heightModifier)`***

This is an overload for previous method. Instead of sending in the count modifier, we just send in the number. Remember, when it's used for placed features, this number represents the vein size per vein.

Example usage:
```java
List<PlacementModifier> modifiers = Registers.Datagen.modifiers(5,
                                                                HeightRangePlacementModifier.uniform(YOffset.fixed(50), 
                                                                                                     YOffset.fixed(60)));
```

---
---
> ##### ***`modifiersWithRarity(int chance, PlacementModifier heightModifier)`***

This method is related to world generation and datagen related to spawning different world features that should use rarity instead of fixed number. The higher the chance number, the lower the rarity and more commong the feature will be.

Example usage:
```java
List<PlacementModifier> modifiers = Registers.Dategen.modifiersWithRarity(10,
                                                                HeightRangePlacementModifier.uniform(YOffset.fixed(50), 
                                                                                                     YOffset.fixed(60)));
```
