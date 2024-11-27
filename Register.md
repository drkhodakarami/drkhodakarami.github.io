## Ji Register

The [Register](https://github.com/drkhodakarami/JiRegister) library. This library contains one class and three interfaces.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiRegister) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/register/) repository for the project.

## IArmorFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering armor items into the Minecraft's registry system.

## IBlockItemFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering block items into the Minecraft's registry system.

## IToolFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering tool items into the Minecraft's registry system.

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

This sub class handles registering every `item` into the Minecraft's registry system.

---
> ##### ***`register(String name)`***

If you want to simply register a simple item with default settings, you can use this method. The method will register an instance of item class with given name and default settings.

Example usage:
```java
Item SOME_ITEM = Registers.Item.register("some_item");
```

---
> ##### ***`register(String name, List<Text> tooltips)`***

If you want to register a simple item with default settings but with the addition of tooltips, you can use this method. The method will register an instance of item class with given name and tooltops and default settings.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_ITEM = Registers.Item.register("some_item", tooltips);
```

---
> ##### ***`register(String name, int stackCount)`***

If you want to define the stack size for a simple item with default settings, you can use this method. The method will register an instance of item class with given stack size being added on top of default settings.

Example usage:
```java
Item SOME_ITEM = Registers.Item.register("some_item", 1);
```

---
> ##### ***`register(String name, int stackCount, List<Text> tooltips)`***

If you want to define the stack size and tooltips for a simple item with default settings, you can use this method. The method will register an instance of item class with given stack size being added on top of default settings with addition of tooltips being set for the instance.

Example usage:
```java
List<Text> tooltips = ... //Create the list of tooltips here.
Item SOME_ITEM = Registers.Item.register("some_item", 1, tooltips);
```

---
> ##### ***`register(String name, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class, you can use this method that will accept the custom class as factory.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", CustomItem::new);
```

---
> ##### ***`register(String name, int stackCount, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with definition of stack size, you can use this method call and send the stack size and the item class factory in.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", 1, CustomItem::new);
```

---
> ##### ***`register(String name, int stackCount, Item.Settings settings, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with definition of stack size and your own custom settings, you can call this method. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", 1, new Item.Settings(), CustomItem::new);
```

---
> ##### ***`register(String name, Item.Settings settings, Function<Item.Settings, T> factory)`***

If you want to register an instance of a custom item class with custom settings and default stack size, you can use this method call and provide the settings and the custom class as factory. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
CustomItem SOME_ITEM = Registers.Item.register("some_item", new Item.Settings(), CustomItem::new);
```

---
> ##### ***`registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, IToolFactory<Item.Settings, T> factory)`***

This method can be used to register any tool item. You will send in the material and other parameters of the tool item and the tool class as the factory.

Example usage:
```java
AxeItem SOME_AXE = Registers.Item.registerTool("some_axe", ModToolMaterials.MATRERIAL, 0.5f, 0.5f, AxeItem::new);
HoeItem SOME_Hoe = Registers.Item.registerTool("some_hoe", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, HoeItem::new);
PickaxeItem SOME_PICKAXE = Registers.Item.registerTool("some_pickaxe", ModToolMaterials.MATRERIAL, 0.1f, 0.1f, PickaxeItem::new);
ShovelItem SOME_SHOVEL = Registers.Item.registerTool("some_shovel", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, ShovelItem::new);
SwordItem SOME_SWORD = Registers.Item.registerTool("some_sword", ModToolMaterials.MATRERIAL, 1.5f, -2.0f, SwordItem::new);
CustomTool SOME_TOOL = Registers.Item.registerTool("some_tool", ModToolMaterials.MATRERIAL, 0.0f, 0.0f, CustomTool::new);
```

---
> ##### ***`registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, Item.Settings settings, IToolFactory<Item.Settings, T> factory)`***

If you want to register a tool but instead of using the default settings, you want to set your custom settings, you can use this method. Take note that in the settings section we are not adding `.registryKey(someKey)` that is needed for 1.21.3 and forward. The library will handle it for you so that you don't need to be worried about this new change.

Example usage:
```java
AxeItem SOME_Axe = Registers.Item.registerTool("some_tool", ModToolMaterials.MATERIAL, 0.0f, 0.0f, new Item.Settings(), AxeItem::new);
```

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
> ##### ***`registerArmor(String name, ArmorMaterial material, EquipmentType equipment, Item.Settings settings, IArmorFactory<Item.Settings, T> factory)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerSnackFood(String name, int stackCount, int nutrition, float saturation)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerSnackFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerFood(String name, int stackCount, int nutrition, float saturation)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

### BlockItem Sub Class

This sub class handles registering every `block item` into the Minecraft's registry system.

> ##### ***`register(Block block)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(Block block, IBlockItemFactory<Item.Settings, T> factory)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(Block block, Item.Settings settings, IBlockItemFactory<Item.Settings, T> factory)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

### Recipe Sub Class

This sub class handles registering every custom `recipe` into the Minecraft's registry system.

> ##### ***`register(String name, RecipeSerializer<?> serializer)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(String name, RecipeType<?> recipeType)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

### ComponentType Sub Class

This sub class handles registering every custom `data component` type into the Minecraft's registry system.

> ##### ***`register(String name, UnaryOperator<ComponentType.Builder<T>> buildOperator)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

### StatusEffect Sub Class

This sub class handles registering every custom `status effect` into the Minecraft's registry system.

> ##### ***`register(String name, StatusEffectCategory category, int color, BiFunction<StatusEffectCategory, Integer, StatusEffect> factory)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

### Datagen Sub Class

This sub class does not register anything into the Minecraft's registry system however, it's responsible of adding helper methods related to `datagen`.

> ##### ***`registerAllArmor(ItemModelGenerator generator, Item[] items, ArmorMaterial material)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerArmor(ItemModelGenerator generator, Item item, ArmorMaterial material, EquipmentSlot slot)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item, float min, float max)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerOrientableVariantBlock(BlockStateModelGenerator generator, Block machine, BooleanProperty property)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`registerCubeVariantBlock(BlockStateModelGenerator generator, Block machine, BooleanProperty property)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`buildHumanoid(String name)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`buildHumanoidAndHorse(String name)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, List<PlacementModifier> modifiers)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, PlacementModifier... modifiers)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`register(Registerable<ConfiguredFeature<?, ?>> context, RegistryKey<ConfiguredFeature<?, ?>> key, F feature, FC configuration)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`modifiers(PlacementModifier countModifier, PlacementModifier heightModifier)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`modifiersWithCount(int count, PlacementModifier heightModifier)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```

---
> ##### ***`modifiersWithRarity(int chance, PlacementModifier heightModifier)`***

text

Example usage:
```java
CustomBlock SOME_BLOCK = Registers.Block.register("some_block", CustomBlock::new);
```