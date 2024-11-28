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

An interface for traversing through enum values. This interface provides methods to navigate to the next and previous values in an enum sequence.

Example Usage:
```java
public enum LimitedDirection implements IEnumTraversable<LimitedDirection>
{
    NORTH, EAST, SOUTH, WEST;
  
    public LimitedDirection next()
    {
        return values()[(ordinal() + 1) % values().legth];
    }
  
    public LimitedDirection previous()
    {
        return values()[(ordinal() - 1 + values().legth) % values().legth];
    }
}
```

---
---
> ##### ***`next()`***

Retrieves the next enum value in the sequence. If the current value is the last in the sequence, this method should return the first enum value.

---
---
> ##### ***`previous()`***

Retrieves the previous enum value in the sequence. If the current value is the first in the sequence, this method should return the last enum value.

## IEnumValueCacher

Interface for enums that cache the values of the enum. This interface provides a method to retrieve cached values of an enum type. Implementing enums should ensure that the enum values are cached efficiently to improve performance when accessing them multiple times.

---
---
> ##### ***`values()`***

Retrives the cached values of the enum as an array containing all the values of the enum that have been cached. The caching mechanism should ensure that the values are stored in a way that allows quick retrieval.

## ISync

This interface provides a contract for implementing syncronization mechanism. Classes that implement this interface should define the specific behavior for the `sync` method, which is intended to ensure that data or state is consistent across different parts of a system.

---
---
> ##### ***`sync()`***

Syncronizes the state or data to ensure consistency. This method should be implemented to define the specific synchronization logic required by the implementing class.

## ITickBE

This is an interface for block entities that need to be ticked every game tick. This interface is dedicated for block entities that don't need to sync data between client and server or on any element at the ticking state. If you need to sync data, use `ITickSyncBE` instead.

---
---
> ##### ***`tick()`***

The tick method that will be called every tick (20 times per second).

---
---
> ##### ***`createTicker(World world)`***

A static helper method that creates a ticker for a block entity in the specific world. Normally you should call this method in the block class at `getTicker()` method override to handle the block entity's ticking logic.

Example Usage:
```java
public class SomeBlock extends Block implements BlockEntityProvider
{
    @Override
    public @Nullable <T extends BlockEntity> BlockEntityTicker<T> getTicker(World world, BlockState state, BlockEntityTYpe<T> type)
    {
        return ITickBE.createTicker(world);
    }
}
```

The default implementation will call the tick method on the block entity only if the world is server side. In the client side world, the default implementation returns null preventing any tick logic on the client.

```java
static <T extends BlockEntity> BlockEntityTicker<T> createTicker(World world)
{
    return !world.isClient ? (pworld, pos, state, entity) -> ((ITickBE) entity).tick() : null;
}
```

Remember, unlike many implementations of getTicker method you see on video tutorials or even in many mod github repository codes, you really don't need to send in any data to the tick method on block entity. There will be a blog post about this talk in the wiki.

---
---
> ##### ***`getState(World world, BlockPos pos)`***

This is a helper method that by default, returns a block state at the given position inside the given world. You can call this anywhere in the block entity to get the block state related to any position. By default, if the world is null, the method will return null.

## ITickSyncBE

This is an interface for block entities that require syncronized ticking. This interface extends `ITickBE`, inheriting its tick functionality, and is intended for block entities that need to perform syncronized updates accross multiple instances or with other game elements. Implementing classes should provide specific logic for syncronized ticking, ensuring that updates are consistent and coordinated within the game world. The block entity should not use the `tick` method for custom ticking logic any more because the tick method is used internally. Instead, they can override `onTick` method that is being called from the tick method internally and you should write your ticking logic inside onTick method.

Example Usage:
```java

public class MyBlockEntity extends NoScreenUpdatableBE implements ITickSyncBE
{
    @Override
    public void onTick()
    {
        //Your custom tick logic goes here
    }
}
```

---
---
> ##### ***`getSyncables()`***

Retrieves a list of `ISync` instances associated with this block entity. This method is used to obtain the collection of syncronizable components that are part of the block entity. Each component in the list implements the ISync interface, allowing it to participate in the syncronization process during the tick cycle.

---
---
> ##### ***`onTick()`***

Executes the tick logic for this block entity. This method is called during each tick cycle to perform any necessary updates or operations for the block entity. Implementers should define the specific actions that need to occure during each tick, such as updating state, interacting with other entities, calling `update` method or managing resources.

The `onTick` method is crucial for maintaining the dynamic behavior of the block entity within the game world. It ensures that the entity's state is consistently updated in response to game events or changes in the environment. Implementers should ensure that the tick logic is efficient and does not introduce performance bottlenecks, as this method is called frequently.

---
---
> ##### ***`tick()`***

Performs the default tick operation for this block entity. Normally you shouldn't override this method unless you want to change how the order of operations for ticking and syncronization logic is handled. This method is invoked during each tick cycle. First it will execute the `onTick` method. After the execution of the method, it will retrieve all the syncable elements by calling `getSyncables` method, and if there is any syncable element added to the block entity, it will call the `sync` method on each one of them. At the end, the method will check if the block entity is instance of `NoScreenUpdateBE` or any of it's inherited children. If so, it will call the `update` method on the block entity to sync the information of the block entity itself between client and server.

## NBTSerializable

An interface that allows for easy serialization and deserialization of objects to NBT. Any element that implements this interface should override `writeNBT` and `readNBT` to handle serialization and deserialization of itself.

---
---
> ##### ***`writeNbt(RegistryWrapper.WrapperLookup registryLookup)`***

Serializes the current state of the implementing object into an NBT element. This method is crucial for converting the object's data into a structured format that can be easily stored, transferred, and reconstructed within the Minecraft environment using the NBT system. The serialization process involves encoding the object's properties and any relevant metadata into an NBT element, which is a hierarchical data structure used extensively in Minecraft for data storage and manipulation.
 
Implementers of this method should ensure that all relevant fields and properties of the object are included in the NBT element. Additionally, care should be taken to maintain compatibility with the NBT format and any versioning requirements that may exist within the application.

---
---
> ##### ***`readNbt(T nbt, RegistryWrapper.WrapperLookup registryLookup)`***

Deserializes the state of the implementing object from the provided NBT element. This method is responsible for reconstructing the object's state by extracting and interpreting the data stored within the NBT element. It is a critical part of the serialization process, allowing objects to be restored to their original state after being saved or transmitted. The deserialization process involves reading the hierarchical data structure of the NBT element and mapping its contents back to the object's fields and properties. This ensures that the object can be accurately reconstructed with all its original data intact.

Implementers of this method should ensure that all relevant fields and properties of the object are correctly populated from the NBT element. Additionally, care should be taken to handle any potential discrepancies or versioning issues that may arise from changes in the NBT format or the object's structure over time.

## BlockPos Payload

A custom payload for sending a `BlockPos` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `BlockPosPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Float Payload

A custom payload for sending a `fload` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `FloatPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Hand Payload

A custom payload for sending a player's `hand` information over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `HandPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Integer Payload

A custom payload for sending an `integer` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `IntegerPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## SingleStack Payload

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

A custom payload for sending a `string` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `StringPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Double Payload

A custom payload for sending a `double` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `DoublePayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Boolean Payload

A custom payload for sending a `boolean` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type)

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `BooleanPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

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
