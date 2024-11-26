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

###### ***`getKey(String name, RegistryKey<? extends Registry<T>> registryKey)`***

This method will return a RegistryKey for the given type. This value is needed for registering anything in the Minecraft registry system (from 1.21.3).

You can get any registry key like this:

```java
RegistryKey<Block> key = getKey(name, RegistryKeys.BLOCK);
```

Just change the RegistryKeys.BLOCK to anything you need from the RegistryKeys class.

### Block Sub Class

This sub class handles registering every `block` into the Minecraft's registry system.

###### `registerSimple(String name, T block)`
###### `register(String name)`
###### `register(String name, List<Text> tooltips)`
###### `register(String name, Block blockCopy)`
###### `register(String name, Block blockCopy, List<Text> tooltips)`
###### `register(String name, AbstractBlock.Settings settings)`
###### `register(String name, AbstractBlock.Settings settings, List<Text> tooltips)`
###### `register(String name, Function<AbstractBlock.Settings, T> factory)`
###### `register(String name, Block blockCopy, Function<AbstractBlock.Settings, T> factory)`
###### `register(String name, AbstractBlock.Settings settings, Function<AbstractBlock.Settings, T> factory)`
###### `registerStair(String name, Block stateBlock, Block copyBlock)`
###### `registerSlab(String name, Block copyBlock)`
###### `registerButton(String name, BlockSetType blockType, int pressureTicks, Block copyBlock)`
###### `registerPressurePlate(String name, BlockSetType blockType, Block copyBlock)`
###### `registerFence(String name, Block copyBlock)`
###### `FenceGateBlock registerFenceGate(String name, WoodType woodType, Block copyBlock)`
###### `registerWall(String name, Block copyBlock)`
###### `registerDoor(String name, BlockSetType blockType, Block copyBlock)`
###### `registerTrapdoor(String name, BlockSetType blockType, Block copyBlock)`

### Entities Sub Class

This sub class handles registering every `entity` (including block entity) into the Minecraft's registry system.

###### `register(String name, Block block, FabricBlockEntityTypeBuilder.Factory<T> factory)`
###### `register(String name, EntityType.EntityFactory<T> factory)`

### Item Sub Class

This sub class handles registering every `item` into the Minecraft's registry system.

###### `register(String name)`
###### `register(String name, List<Text> tooltips)`
###### `register(String name, int stackCount)`
###### `register(String name, int stackCount, List<Text> tooltips)`
###### `register(String name, Function<Item.Settings, T> factory)`
###### `register(String name, int stackCount, Function<Item.Settings, T> factory)`
###### `register(String name, int stackCount, Item.Settings settings, Function<Item.Settings, T> factory)`
###### `register(String name, Item.Settings settings, Function<Item.Settings, T> factory)`
###### `registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, IToolFactory<Item.Settings, T> factory)`
###### `registerTool(String name, ToolMaterial material, float attackDamage, float attackSpeed, Item.Settings settings, IToolFactory<Item.Settings, T> factory)`
###### `registerArmor(String name, ArmorMaterial material, EquipmentType equipment, IArmorFactory<Item.Settings, T> factory)`
###### `registerArmor(String name, ArmorMaterial material, EquipmentType equipment, Item.Settings settings, IArmorFactory<Item.Settings, T> factory)`
###### `registerSnackFood(String name, int stackCount, int nutrition, float saturation)`
###### `registerSnackFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`
###### `registerFood(String name, int stackCount, int nutrition, float saturation)`
###### `registerFood(String name, int stackCount, int nutrition, float saturation, List<Text> tooltips)`

### BlockItem Sub Class

This sub class handles registering every `block item` into the Minecraft's registry system.

###### `register(Block block)`
###### `register(Block block, IBlockItemFactory<Item.Settings, T> factory)`
###### `register(Block block, Item.Settings settings, IBlockItemFactory<Item.Settings, T> factory)`

### Recipe Sub Class

This sub class handles registering every custom `recipe` into the Minecraft's registry system.

###### `register(String name, RecipeSerializer<?> serializer)`
###### `register(String name, RecipeType<?> recipeType)`

### ComponentType Sub Class

This sub class handles registering every custom `data component` type into the Minecraft's registry system.

###### `register(String name, UnaryOperator<ComponentType.Builder<T>> buildOperator)`

### StatusEffect Sub Class

This sub class handles registering every custom `status effect` into the Minecraft's registry system.

###### `register(String name, StatusEffectCategory category, int color, BiFunction<StatusEffectCategory, Integer, StatusEffect> factory)`

### Datagen Sub Class

This sub class does not register anything into the Minecraft's registry system however, it's responsible of adding helper methods related to `datagen`.

###### `registerAllArmor(ItemModelGenerator generator, Item[] items, ArmorMaterial material)`
###### `registerArmor(ItemModelGenerator generator, Item item, ArmorMaterial material, EquipmentSlot slot)`
###### `customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item, float min, float max)`
###### `customOreDrops(FabricBlockLootTableProvider provider, RegistryWrapper.WrapperLookup registries, Block drop, Item item)`
###### `registerOrientableVariantBlock(BlockStateModelGenerator generator, Block machine, BooleanProperty property)`
###### `registerCubeVariantBlock(BlockStateModelGenerator generator, Block machine, BooleanProperty property)`
###### `buildHumanoid(String name)`
###### `buildHumanoidAndHorse(String name)`
###### `register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, List<PlacementModifier> modifiers)`
###### `register(Registerable<PlacedFeature> context, RegistryKey<PlacedFeature> key, RegistryEntry<ConfiguredFeature<?, ?>> configuration, PlacementModifier... modifiers)`
###### `register(Registerable<ConfiguredFeature<?, ?>> context, RegistryKey<ConfiguredFeature<?, ?>> key, F feature, FC configuration)`
###### `modifiers(PlacementModifier countModifier, PlacementModifier heightModifier)`
###### `modifiersWithCount(int count, PlacementModifier heightModifier)`
###### `modifiersWithRarity(int chance, PlacementModifier heightModifier)`