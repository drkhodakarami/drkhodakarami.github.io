## Ji Reference

The [Reference](https://github.com/drkhodakarami/JiReference) library. This library contains 3 classes. This library contains most of the used references in mods.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiReference) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/reference/) repository for the project.

## BEKeys Class

This class contains some static string values for different keys that can be used in read and write NBT methods for block entities. The values contain:
- ENERGY_AMOUNT = `.be.energy.amount`
- ENERGY_CAPACITY = `.be.energy.capacity`
- HAS_ENERGY = `.be.has.energy`
- FLUID_AMOUNT = `.be.fluid.amount`
- FLUID_CAPACITY = `.be.fluid.capacity`
- HAS_FLUID = `.be.has.fluid`
- PROGRESS_AMOUNT = `.be.progress.amount`
- PROGRESS_MAX = `.be.progress.max`
- COOLDOWN_AMOUNT = `.be.cooldown.amount`
- COOLDOWN_MAX = `.be.cooldown.max`
- BURN_AMOUNT = `.be.burn.amount`
- BURN_MAX = `.be.burn.max`
- HAS_INVENTORY = `.be.has.inventory`

## DirectionSuffix Class

This class contains some static string values that can be used as suffix for directions to be added onto your original keys. The values contain:
- NORTH = `.north`
- SOUTH = `.south`
- WEST = `.west`
- EAST = `.east`
- TOP = `.top`
- BOTTOM = `.bottom`
- NO_DIRECTION = `.no.direction`

## JiReference Class

This is the most important file in this library. It's a class that you can directly instantiate in your mod or if you want to add more information on top of what is provided, you can add your custom class the extends this class. This class provides a static `GSON` attribute that is already set and ready to be used for custom serialization of information into json files or objects. If you want to directly use this class in your mod you should instantiate it in the mod's main entry file (or any other place but the attribute should be static. Alternatively, you will instantiate your custom class the extends this one).

```java
public class Main implements ModInitializer
{
    public static String ModID = "your_mod_id";
    public static JiReference REFERENCE = new JiReference(ModID);
}
```

---
---
> ##### ***`ModID()`***

Returns the mod's id that was used when instantiated this class.

---
---
> ##### ***`identifier(@NotNull String path)`***

Returns an identifier that is using your mod id as the namespace.

---
---
> ##### ***`vanillaID(@NotNull String path)`***

Returns an identifier that is using Minecraft's vanilla namespace.

---
---
> ##### ***`idOf(@NotNull String path)`***

This method will try to parse any given string into an identifier and return the result identifier.

---
---
> ##### ***`translate(String key, Object... params)`***

The method returns a `MutableText` that is formated with `modId.` before the key. It also accepts any number of parameters to be used in the translation file to be replaced by code.

---
---
> ##### ***`translateContainer(String key, Object... params)`***

The method returns a `MutableText` that is formated with `modId.container.` before the key. It also accepts any number of parameters to be used in the translation file to be replaced by code. It is specifically used to add a translation key for any block entity container.

---
---
> ##### ***`createBlockTag(String name)`***

This method returns a `TagKey<Block>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified by your mod id's namespace.

---
---
> ##### ***`createBlockCommonTag(String name)`***

This method returns a `TagKey<Block>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified under `c` namespace that is a standard used by Fabric MC for common tags.

---
---
> ##### ***`createItemTag(String name)`***

This method returns a `TagKey<Item>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified by your mod id's namespace.

---
---
> ##### ***`createItemCommonTag(String name)`***

This method returns a `TagKey<Item>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified under `c` namespace that is a standard used by Fabric MC for common tags.

---
---
> ##### ***`createEntityTag(String name)`***

This method returns a `TagKey<EntityType<?>>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified by your mod id's namespace.

---
---
> ##### ***`createEntityCommonTag(String name)`***

This method returns a `TagKey<EntityType<?>>` with the given name that can be used as a tag in Minecraft's tag system. The tag is identified under `c` namespace that is a standard used by Fabric MC for common tags.

---
---
> ##### ***`energyAmountKey()`***

Returns the `ENERGY_AMOUNT` key with your mod id being added at the start of the string.

---
---
> ##### ***`energyCapacityKey()`***

Returns the `ENERGY_CAPACITY` key with your mod id being added at the start of the string.

---
---
> ##### ***`hasEnergyKey()`***

Returns the `HAS_ENERGY` key with your mod id being added at the start of the string.

---
---
> ##### ***`fluidAmountKey()`***

Returns the `FLUID_AMOUNT` key with your mod id being added at the start of the string.

---
---
> ##### ***`fluidCapacityKey()`***

Returns the `FLUID_CAPACITY` key with your mod id being added at the start of the string.

---
---
> ##### ***`hasFluidKey()`***

Returns the `HAS_FLUID` key with your mod id being added at the start of the string.

---
---
> ##### ***`progressAmountKey()`***

Returns the `PROGRESS_AMOUNT` key with your mod id being added at the start of the string.

---
---
> ##### ***`maxProgressKey()`***

Returns the `PROGRESS_MAX` key with your mod id being added at the start of the string.

---
---
> ##### ***`cooldownAmountKey()`***

Returns the `COOLDOWN_AMOUNT` key with your mod id being added at the start of the string.

---
---
> ##### ***`maxCooldownKey()`***

Returns the `COOLDOWN_MAX` key with your mod id being added at the start of the string.

---
---
> ##### ***`burnAmountKey()`***

Returns the `BURN_AMOUNT` key with your mod id being added at the start of the string.

---
---
> ##### ***`maxBurnKey()`***

Returns the `BURN_MAX` key with your mod id being added at the start of the string.

---
---
> ##### ***`hasInventoryKey()`***

Returns the `HAS_INVENTORY` key with your mod id being added at the start of the string.