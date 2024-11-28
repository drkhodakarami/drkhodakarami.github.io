## Jira Lib

The [JiraLib](https://github.com/drkhodakarami/JiraLib) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle network sync, inventory, energy, or fluid. It also provides nice payloads and some records that can be used in other projects. It contains 13 classes, 8 interfaces, 5 network payloads, and 3 records. There are 4 other libraries that are depending on this library [JiFluid](https://github.com/drkhodakarami/JiFluid), [JInventory](https://github.com/drkhodakarami/JInventory), [JiEnergy](https://github.com/drkhodakarami/JiEnergy), [JiMachina](https://github.com/drkhodakarami/JiMachina).

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, when we talk about different payload records bellow, you should know that these records **has** the ability to be used as a payload. All of these records are providing a `Codec` and because of this, they can be used as payloads, normal records, or even custom `DataComponentType`.
</div>

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

A custom payload for sending a `BlockPos` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

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

## Boolean Payload

A custom payload for sending a `boolean` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

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

## CoordinateData Payload

A custom payload for sending a `BlockPos` and a `String` (dimension) over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the normal use case for this record, is to use this record as custom component data. The ability to use this record as a payload is extra functionality.
</div>

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `CoordinateDataPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

## Double Payload

A custom payload for sending a `double` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

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

## Float Payload

A custom payload for sending a `fload` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

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

## FluidStack Payload

A custom payload for sending a `FluidVariant` and a `long` (fluid amount) over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `FluidStackPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

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

## OutputItemStack Payload

A custom payload for sending an `item`, `integer` (item count), and `FloatProvider` (chance) over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the normal use case for this record, is to provide an output item stack that can be outputted from a machine using a chance value for production. The ability to use this record as a payload is extra functionality.
</div>

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

A custom payload for sending a `ItemStack` over the network. Although the record is named as payload, it provides a CODEC and can be used for other perpouses utilizing this codec (for example a custom data type).

---
---
> ##### ***`ID`***

The static ID field of a single payload.

---
---
> ##### ***`CODEC`***

A `Codec` for serializing and deserializing instances of `SingleStackPayload`. This codec utilizes the `RecordCodecBuilder` to define the structure of the payload object for encoding and decoding operations.

---
---
> ##### ***`PACKET_CODEC`***

The static `PacketCodec` field of a single payload used for handling serialization for the network in Minecraft system.

---
---
> ##### ***`getId()`***

This method returns the `ID` for a payload.

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

## PosHelper Class

This is a helper class that provides static helper methods all related to block position handling.

---
---
> ##### ***`positionDirectTo(BlockPos pos, Direction direction)`***

Returns the `BlockPos` with offset of one block from the given position in the given direction.

---
---
> ##### ***`positionNextTo(BlockPos pos)`***

Returns an array of `BlockPos` that are one block in each direction of the given position.

---
---
> ##### ***`positionSideTo(BlockPos pos)`***

Returns an array of `BlockPos` that are one block in each direction of the given position with the exception of top and bottom ones.

---
---
> ##### ***`positionNextNotTop(BlockPos pos)`***

Returns an array of `BlockPos` that are one block in each direction of the given position with the exception of top one.

---
---
> ##### ***`positionNextNotBottom(BlockPos pos)`***

Returns an array of `BlockPos` that are one block in each direction of the given position with the exception of bottom one.

---
---
> ##### ***`left(Direction facing)`***

Returns the `Direction` that is to the **left** of the given `facing` direction.

---
---
> ##### ***`right(Direction facing)`***

Returns the `Direction` that is to the **right** of the given `facing` direction.

---
---
> ##### ***`front(Direction facing)`***

Returns the `Direction` that is to the **front** of the given `facing` direction.

---
---
> ##### ***`back(Direction facing)`***

Returns the `Direction` that is to the **back** of the given `facing` direction.

---
---
> ##### ***`leftBlock(BlockPos pos, Direction facing)`***

Returns the `BlockPos` that is to the **left** of the given `facing` direction at the given position.

---
---
> ##### ***`rightBlock(BlockPos pos, Direction facing)`***

Returns the `BlockPos` that is to the **right** of the given `facing` direction at the given position.

---
---
> ##### ***`backBlock(BlockPos pos, Direction facing)`***

Returns the `BlockPos` that is to the **back** of the given `facing` direction at the given position.

---
---
> ##### ***`frontBlock(BlockPos pos, Direction facing)`***

Returns the `BlockPos` that is to the **front** of the given `facing` direction at the given position.

---
---
> ##### ***`topBlock(BlockPos pos)`***

Returns the `BlockPos` that is to the **top** of the given `facing` direction at the given position.

---
---
> ##### ***`bottomBlock(BlockPos pos)`***

Returns the `BlockPos` that is to the **bottom** of the given `facing` direction at the given position.

## MathHelper Class

This is a helper class that provides static helper methods with various math functions.

---
---
> ##### ***`getChance(int value)`***

Returns a random chance float number based on the given value. In other words, if you provide `5` as the `value` the result will be `0.95` as a float.

---
---
> ##### ***`getChance(World world, int value)`***

This method uses the previous one to produce a random boolean true or false result based on the value. The method will use the random number generator provided by the world you send to the method. For example, if you send `5` as the value, the method will try to generate a random float number between 0 and 1. Only if the generated number is greater than or equal to `0.95` the method will return `true`. If the generated number is smaller than 0.95, the method will return `false`. The result is a perfect `chance` system that you can use anywhere is your code.

## ExtraPacketCodecs Class

This class in a helper class with static helper methods that will help you encode and decode `ByteBuf` using `integer provider type` and `float provider type`.

---
---
> ##### ***`registerIntProviderCodec(IntProviderType<T> type, PacketCodec<RegistryByteBuf, T> codec)`***

This method will register a codec for an `IntProviderType` and add it to the internal cache for all the registered integer provider types.

---
---
> ##### ***`registerFloatProviderCodec(FloatProviderType<T> type, PacketCodec<RegistryByteBuf, T> codec)`***

This method will register a codec for a `FloatProviderType` and add it to the internal cache for all the registered fload provider types.

---
---
> ##### ***`getIntProviderCodec(IntProviderType<?> type)`***

Retrieves a `PacketCodec<RegistryByteBuf, ? extends IntProvider>` from the integer provider type that is cached internally in the `Map` of cached integer provider types.

---
---
> ##### ***`getFloatProviderCodec(FloatProviderType<?> type)`***

Retrieves a `PacketCodec<RegistryByteBuf, ? extends FloatProvider>` from the float provider type that is cached internally in the `Map` of cached float provider types.

---
---
> ##### ***`encode(ByteBuf buf, T intProvider)`***

This method encodes an `IntProvider` into a `ByteBuf`.

---
---
> ##### ***`decode(ByteBuf buf, IntProviderType<T> type)`***

This method decodes an `IntProvider` from a `ByteBuf`.

---
---
> ##### ***`encode(ByteBuf buf, T floatProvider)`***

This method encodes an `FloatProvider` into a `ByteBuf`.

---
---
> ##### ***`decode(ByteBuf buf, FloatProviderType<T> type)`***

This method decodes an `FloatProvider` from a `ByteBuf`.

---
---
> ##### ***`registerDefaults()`***

Registers the default codecs for all `IntProviderType` and `FloatProviderType`. The types that are registered are:
- IntProviderType.CONSTANT
- IntProviderType.UNIFORM
- IntProviderType.BIASED_TO_BOTTOM
- IntProviderType.CLAMPED
- IntProviderType.WEIGHTED_LIST
- IntProviderType.CLAMPED_NORMAL
- FloatProviderType.CONSTANT
- FloatProviderType.UNIFORM
- FloatProviderType.CLAMPED_NORMAL
- FloatProviderType.TRAPEZOID

---
---
> ##### ***`weightedListCodec(PacketCodec<B, E> elementCodec)`***

Creates a codec for a weighted list of elements. The method returns `<B extends ByteBuf, E> PacketCodec<B, DataPool<E>>`.

## MouseHelper Class

This is a utility class for handling mouse interactions and provides static helper methods to handle mouse cursure on client side.

---
---
> ##### ***`isMouseOver(double mouseX, double mouseY, int x, int y, int width, int height, int offsetX, int offsetY)`***

Checks if the mouse cursure is over a boundary. The boundary is provided by top left corner, width and height of the boundary and the offset values for x and y directions.

---
---
> ##### ***`isMouseOver(double mouseX, double mouseY, int x, int y)`***

Checks if the mouse cursure is over a boundary. The boundary is provided by top left corner. This method uses fixed values of 16 pixels for both width and height of the boundary. This method will not accept any offset values for the rectangular area.

---
---
> ##### ***`isMouseOver(double mouseX, double mouseY, int x, int y, int size)`***

Checks if the mouse cursure is over a boundary. The boundary is provided by top left corner, and the same size value for both width and height of the boundary. This method will not accept any offset values for the rectangular area.

---
---
> ##### ***`isMouseOver(double mouseX, double mouseY, int x, int y, int width, int height)`***

Checks if the mouse cursure is over a boundary. The boundary is provided by top left corner, width and height of the boundary. This method will not accept any offset values for the rectangular area.

## InfoArea Class

This `abstract` class extends `DrawContext` to provide a specialized area for rendering information within a specific rectangular region. The base usage of this class is to render fluid/energy inside the GUI of a container. 

---
---
> ##### ***`InfoArea(MinecraftClient client, VertexConsumerProvider.Immediate vertexConsumers)`***

The base constructor that will set the `Rect2i` area as zero with, height, and in the top left (0, 0) coordinates.

---
---
> ##### ***`InfoArea(MinecraftClient client, VertexConsumerProvider.Immediate vertexConsumers, Rect2i area)`***

The constructor that will set the internal `Rect2i` with the provided area values.

---
---
> ##### ***`draw()`***

Draws the content withing the specific `DrawContext`. This method is intended to be overridden to provide custom drawing logic within the defined area. By default this method does not perform any drawing operation.

## NoScreenUpdatableBE Class

Represents an `abstract` base class for a `block entity` that requre periodic `update` to sync information between client and server. This class does not handle any GUI screens and is suited for those block entities that should not have any user interface. If you need the same functionality but with the handling of GUI screens, take a loot at `UpdatableBE` class. There are three classes that are extending from this base class: `NoScreenUpdatableEndTickBE`, `UpdatableBE`, and `UpdatableEndTickBE`.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, under normal conditions, you don't need to override the update method. By default, the <strong>update method</strong> will sync the information between the server and client. Anytime that you would call markDirty on a normal block entity, now you should call the update method to handle both marking the block entity dirty, and syncronizing the information between server and client. If you are extending your block entity form this class or any other class that is inherited from this one, you don't need to send custom packets from server to client to sync information any more.
</div>

---
---
> ##### ***`NoScreenUpdatableBE(BlockEntityType<?> type, BlockPos pos, BlockState state)`***

The block entity constructor that mimics the vanilla constructor. If you want to have your custom constructor that is not accepting the type but reading it directly from a static constant attribute on a class, you can use the example bellow. In this example, we assume that you are registering a `BlockEntityType<?>` inside the `ModEntities` class as a static field attribtue.

```java
public NoScreenUpdatableBE(BlockPos pos, BlockState state)
{
    this(ModEntities.SOME_BLOCK_ENTITY, pos, state);
}
```

---
---
> ##### ***`update()`***

By default because we are returning `false` in the `shouldWaitEndTick`, the update function will mark the block entity as `dirty`, and then it will syncronize the information between server and client with a call to `updateListeners` method on the `world` object with `NOTIFY_ALL` flag. The result is not only syncronization of the data between server and client but the update listeners method handles some extra functionality that you can read from vanilla code yourself.

Keep in mind, in the `EndTickBE` classes that inherit from this base class, we return `true` for the `shouldWaitEndTick` method. In those classes, the only thing that will happen in this method, is to mark the block entity for the one that is waiting for an update call and the update logic and syncroniztion will handled by the `endTick` method at the end of the tick cycle.

---
---
> ##### ***`shouldWaitEndTick()`***

This method will return a flag called that will indicate if the update should be deferred for the end of the tick cycle. By default, in this class it will return false. If you want the deferred functionality, take a look at `NoScreenUpdatableEndTickBE` class.

---
---
> ##### ***`endTick()`***

This is the base functionality implementation  ones to be used by the inherited `EndTickBE` classes. Basically, this method will check if we marked the block entity for update or not, if yes, then it will mark the block entity dirty and call the updateListeners.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, to find the use case of the end tick call, you can take a look at <strong>tick</strong> method inside <strong>ITickSyncBE</strong> interface. In this interface, after syncronizing every syncable object, we check if the block entity is instance of <strong>NoScreenUpdatableBE</strong> and if it's the case, we call the endTick() on the block entity. The beauty of this system is that, if you block entity is not called for an <strong>update</strong> at this point, the endTick will do nothing. However, if you have called for the update any place in the onTick method and the class is marked to return true for <strong>shouldWaitEndTick</strong>, now it will call for the update logic at the execution of <strong>endTick</strong> method. The same approach can be used at your will whenever you think you need to handle the update at the end of tick cycle. Just remember, other than the mentioned example, there is no other usages in the library and if you are implementing your own logic separated from ITickSyncBE, you need to call the endTick manually yourself when you see appropriate.
</div>

---
---
> ##### ***`toUpdatePacket()`***

Creates a packet that is sent to the client to update the block entity's state. This packet includes all necessary data to syncronize the client with the current state of the block entity on the server. Normally, you shouldn't need to override this method any more because the base class handles the functionality for you.

---
---
> ##### ***`toInitialChunkDataNbt(RegistryWrapper.WrapperLookup registries)`***

Converts the initial chunk data of this block entity into an NBT compound and calls the writeNbt with this compound. This allows the block entity to serialize its initial state for sending to clients when the chunk is loaded. This includes the inventory and other relative data necessary for the client to replicate the block entity's state accurately. Normally, you shouldn't need to override this method any more because the base class handles the functionality for you.

## NoScreenUpdatableEndTickBE Class

This class extends the `NoScreenUpdatableBE` and the only main difference is that it overrides the `shouldWaitEndTick` that returns `true` by default. By returning true in the method, we mark this block entity as the one that should handle the update call at the end of tick cycle.

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
