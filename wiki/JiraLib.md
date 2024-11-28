## Jira Lib

The [JiraLib](https://github.com/drkhodakarami/JiraLib) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle network sync, inventory, energy, or fluid. It also provides nice payloads and some records that can be used in other projects. It contains 13 classes, 8 interfaces, 5 network payloads, and 3 records. There are 4 other libraries that are depending on this library [JiFluid](https://github.com/drkhodakarami/JiFluid), [JInventory](https://github.com/drkhodakarami/JInventory), [JiEnergy](https://github.com/drkhodakarami/JiEnergy), [JiMachina](https://github.com/drkhodakarami/JiMachina).

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiraLib) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/reference/) repository for the project.

## IIngredientRenderer

This is an interface used for rendering ingredients in a graphical user interface. It defines methods for rendering, retrieving tooltips, and obtaining font renderer from the text that is associated with the ingredients.

---
---
> ##### ***`render(MatrixStack stack, T ingredient)`***

This method is for rendering the ingredient at the default position of (0, 0). The 0,0 position is at the top left corner of the GUI. The default implementation is calling another overload of the same method that accepts x and y coordinates.

---
---
> ##### ***`render(MatrixStack stack, int x, int y, T ingredient)`***

This method is for rendering the ingredient at the given x and y coordinates inside a GUI. The ingredient can be null, in which case no rendering should occure.

---
---
> ##### ***`tooltip(T ingredient, Item.TooltipContext tooltipFlag, String modid)`***

This method is for getting the list of text components as tooltip from the ingredient.

---
---
> ##### ***`fontRenderer(MinecraftClient client, T ingredient)`***

This method gets the font renderer used for rendering text associated with the ingredient. By default it returns the text renderer from the provided Minecraft client instance.

---
---
> ##### ***`width()`***

This method returns the width of the rendering ingredient. By default it returns a fixed width of 16 pixels.

---
---
> ##### ***`height()`***

This method returns the height of the rendering ingredient. By default it returns a fixed width of 16 pixels.

## IEnumStringRepresentable

An interface for enums that can be represented as a string. This interface provides methods to retrieve enum constants based on their string representation. It is useful for converting between enums and their string names, especially when dealing with serialization of user input. Implementing classes should ensure that the `getSerializedName` method returns a unique string for each enum constant.

---
---
> ##### ***`getEnumByName(Class<T> enumClass, String serializedName)`***

Returns the enum object represented by the string. This method attempts to find an enum constant within the specific enum class that matches the given string representation.

---
---
> ##### ***`getEnumByName(T[] enumConstants, String serializedName)`***

Returns the enum object represented by the string. This method iterates over the provided array of enum constants to find a match for given string representation.

---
---
> ##### ***`getSerializedName()`***

Retrices the eunique string representation of this enum constant. This method is intended to provide a consistent and unique identifier for each enum constant, which can be used for serialization, deserialization, or display purposes.

## IEnumTraversable

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## IEnumValueCacher

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## ISync

text

---
---
> ##### ***``***

text

## ITickBE

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## ITickSyncBE

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## NBTSerializable

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## BlockPos Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## Float Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## Hand Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## Integer Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## String Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## Double Payload

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## CoordinateData Record

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## FluidStack Record

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## OutputItemStack Record

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## PosHelper Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## MathHelper Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## ExtraPacketCodecs Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## MouseHelper Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## InfoArea Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## NoScreenUpdatableBE Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## NoScreenUpdatableEndTickBE Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## UpdatableBE Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## UpdatableEndTickBE Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## AbstractActivatableBlock Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## AbstractFacingBlock Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## AbstractHorizontalFacingBlock Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## BlockWithBE Class

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text
