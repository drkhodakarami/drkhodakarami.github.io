## Ji Energy

The [JiEnergy](https://github.com/drkhodakarami/JiEnergy) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle energy containers. It provides abstract classes, wrapped energy containers, and helper methods that will handle more complex energy transfer for you. It contains 5 classes, and 1 interfaces. The [JiMachina](https://github.com/drkhodakarami/JiMachina) library is depending on this one.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractEnergyBE</strong>, extend your block from <strong>AbstractEnergyBlock</strong> and use helper methods provided by EnergyHelper class.
</div>

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiEnergy) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/energy/) repository for the project.

## IWrappedEnergyProvider

The {@code IWrappedEnergyProvider} interface defines the contract for classes that provide energy storage capabilities using a {@link WrappedEnergyStorage}. It includes methods to add energy storage with specified parameters and to retrieve the current energy storage. Implementations of this interface are expected to manage energy storage configurations and provide access to the underlying {@link WrappedEnergyStorage}.

---
---
> ##### ***`AddEnergyStorage(int capacity, int maxInsert, int maxExtract, Direction direction)`***

Adds a new energy storage unit with the specified capacity, maximum insertion rate, maximum extraction rate, and direction.

---
---
> ##### ***`getEnergyStorage()`***

Retrieves the current energy storage associated with this block entity. This method allows other components of the system to access the energy storage configuration defined for this block entity. It returns an instance of `WrappedEnergyStorage` containing syncronized energy storage units, which can be used for operations related to energy management, such as checking current energy levevls or manipulating energy within the storage.

## SyncedEnergyStorage Class

The `SyncingEnergyStorage` class extends `SimpleEnergyStorage` to provide energy storage capabilities with synchronization between server and client. It utilizes a `NoScreenUpdatableBE` block entity to ensure that any changes in energy storage are properly updated across the network.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the class is implementing <strong>ISync</strong> interface and implements a default system that will check if the storage is marked as dirty. If so, it will check if the block entity associated to the storage is of type `NoScreenUpdatableBE` or any of it's children or not. If it is, the sync method will call the <strong>update</strong> method on the block entity and if it's not an instance of the said class, the sync method will simply call the <strong>markDirty</strong> method of the block entity. Normally you shouldn't need to ovevrride the <strong>sync</strong> method but if for any reason you need to override this method, don't forget to call the super to make sure this default behavior is maintained.
</div>

---
---
> ##### ***`sync()`***

Synchronizes the energy storage state with the client and updates the associated block entity. This method is responsible for ensuring that the current state of the energy storage is accurately reflected on the client side. It performs necessary operations to update the `BlockEntity` and any other components that rely on the energy storage data. Implementations should leverage the `ISync` interface to facilitate efficient data synchronization across the network. This method is typically called whenever there are changes in the energy storage that need to be communicated to the client, ensuring consistency between the server and client states.

---
---
> ##### ***`onFinalCommit()`***

This method is called when the changes to the energy storage are finalized. It is responsible for performing necessary actions after energy storage modifications are committed. The method first invokes the parent class's `SimpleEnergyStorage#onFinalCommit` to ensure that any inherited finalization logic is executed. After that, it marks the storage as dirty to be synchronized on the `sync` method call.

---
---
> ##### ***`getBlockEntity()`***

Retrieves the block entity associated with this energy storage.

## WrappedEnergyStorage Class

Theis class is a generic wrapper for managing multiple `SimpleEnergyStorage` instances. It provides methods to add, retrieve, and manage energy storages, including direction-specific storages. It also implements the `NBTSerializable` interface for serialization and deserialization of energy storage data to and from NBT format.

---
---
> ##### ***`addStorage(SimpleEnergyStorage storage)`***

Adds a new energy storage without specifying a direction.

---
---
> ##### ***`addStorage(SimpleEnergyStorage storage, Direction direction)`***

Adds a new storage and associates it with a specific direction.

---
---
> ##### ***`addSimpleStorage(int capacity, int maxInsert, int maxExtract)`***

Adds a new `SimpleEnergyStorage` to the specific block entity with the given capacity, max insert rate and max extract rate.

---
---
> ##### ***`addSimpleStorage(int capacity, int maxInsert, int maxExtract, Direction direction)`***

Adds a new `SimpleEnergeStorage` to the specific block entity with the given paramets and associates it with a specific direction.

---
---
> ##### ***`addSyncedStorage(NoScreenUpdatableBE blockEntity, int capacity, int maxInsert, int maxExtract)`***

Adds a new `SyncedEnergyStorage` to the specific block entity with the given capacity, max insert and extract rates.

---
---
> ##### ***`addSyncedStorage(NoScreenUpdatableBE blockEntity, int capacity, int maxInsert, int maxExtract, Direction direction)`***

Adds a new `SyncedEnergyStorage` to the specific block entity with the given parameters and associates it with a specific direction.

---
---
> ##### ***`getStorages()`***

Retrieves a list of all energy storages wrapped in this storage.

---
---
> ##### ***`getSidedStorageMap()`***

Retrieves a `HashMap` of direction-specific storages.

---
---
> ##### ***`getAmount(Direction direction)`***

Retrieves the amount of energy stored in the storage associated with the specified direction.

---
---
> ##### ***`getCapacity(Direction direction)`***

Retrieves the capacity of the storage associated with the specified direction.

---
---
> ##### ***`getMaxInsert(Direction direction)`***

Retrieves the maximum amount of energy that can be insertable into the storage associated with the specified direction.

---
---
> ##### ***`getMaxExtract(Direction direction)`***

Retrieves the maximum amount of energy that can be extractable from the storage associated with the specified direction.

---
---
> ##### ***`getAmount(int index)`***

Retrieves the amount of energy stored in the storage at the specified index.

---
---
> ##### ***`getCapacity(int index)`***

Retrieves the capacity of the storage at the specified index.

---
---
> ##### ***`getMaxInsert(int index)`***

Retrieves the maximum amount of energy that can be insertable into the storage at the specified index.

---
---
> ##### ***`getMaxExtract(int index)`***

Retrieves the maximum amount of energy that can be extractable from the storage at the specified index.

---
---
> ##### ***`getStorage(@Nullable Direction direction)`***

Retrieves the storage associated with the specified direction.

---
---
> ##### ***`getStorage(int index)`***

Retrieves the storage at the specified index.

---
---
> ##### ***`writeNbt(RegistryWrapper.WrapperLookup registryLookup)`***

Serializes the energy storage data to an NBT list. This method is used to save the current state of the energy storages so that it can be restored later.

---
---
> ##### ***`readNbt(NbtList nbt, RegistryWrapper.WrapperLookup registryLookup)`***

Deserializes the energy storage data from an NBT list. This method is used to restore the energy storage state from saved data.

---
---
> ##### ***`getProvider(Direction direction, Direction facing)`***

Retrieves a `SimpleEnergyStorage` related to a direction using the facing of the block.

Example Usage:
```java
WrappedEnergyStorage energyStorage =...; //Creating an energy storage
SingleEnergyStorage battery = energyStorage.getProvider(Direction.NORTH, getCachedState().get(SomeBlock.FACING));
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the <strong>direction</strong> that you send in to retrieve the storage, should be the same direction that you assigned when creating the storage and adding it to the wrapped storage. In other words, if the energy storage should be on the east side of the block because of it's rotation in the world, you should completely ignore this fact and simply use the same direction that you used to register the storage into the wrapped container (in this example we had originally used north). The easy way to understand what needs to be done, is to put the block in such a way that <strong>FACING</strong> property will return <strong>north</strong>. Then consider what should be on each side of the container and register the storages with the proper direction at that placed configuration. From that point forward, you can assume that the whole system will work even when you rotate the block by facing property.
</div>

## EnergyHelper Class

A helper class that provides static helper method that can handle spreading energy to adjacent blocks, retrieving a possible energy storage on a single direction, or retrieving all the possible energy storages around the block.

---
---
> ##### ***`simulateInsertion(EnergyStorage storage, long amount, Transaction outer)`***

Simulates the insertion of a specified amount of energy into an `EnergyStorage`. This method opens a nested transaction to determine the maximum amount of energy that can be inserted without actually committing the transaction.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, SimpleEnergyStorage storage)`***

Spreads energy from a `SimpleEnergyStorage` to adjacent `EnergyStorage` instances in the world, It checks each direction for available energy storage and attempts to distribute energy evenly. If the energy amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, SimpleEnergyStorage storage, boolean equalAmount)`***

Spreads energy from a `SimpleEnergyStorage` to adjacent `EnergyStorage` instances in the world, It checks each direction for available energy storage and attempts to distribute energy. The distribution can be equal or max possible per side, depending on the `equalAmount` flag. If the energy amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Set<BlockPos> exceptions, SimpleEnergyStorage storage)`***

Spreads energy from a `SimpleEnergyStorage` to adjacent `EnergyStorage` instances in the world, excluding specific positions defined in the exceptions set. It checks each direction for available energy storage and attempts to distribute energy evenly. If the energy amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Set<BlockPos> exceptions, SimpleEnergyStorage storage, boolean equalAmount)`***

Spreads energy from a `SimpleEnergyStorage` to adjacent `EnergyStorage` instances in the world, excluding specific positions defined in the exceptions set. It checks each direction for available energy storage and attempts to distribute energy. The distribution can be equal or max possible per side, depending on the `equalAmount` flag. If the energy amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`getEnergyStorage(World world, BlockPos pos, Set<BlockPos> exceptions, Direction direction)`***

Retrieves the energy storage at a specified position in the world, considering a given direction and a set of exceptions. This method attempts to access the energy storage located at the specified `BlockPos` within the provided `World`. It takes into account the direction from which the storage is accessed, allowing for directional energy interactions. Additionally, it considers a set of exception positions, which are locations that should be ignored during the search for energy storage. This can be useful in scenarios where certain blocks should not be interacted with, such as when avoiding recursive or redundant checks.

---
---
> ##### ***`getAllEnergyStorages(World world, BlockPos pos, Set<BlockPos> exceptions)`***

Retrieves all energy storages surrounding a specified position in the world, excluding specified exceptions. This method scans the area around the given `BlockPos` within the provided `World` to find all available energy storages. It returns a list of energy storages that are accessible from the specified position, while excluding any positions listed in the `exceptions` set. This is useful for operations that require interaction with multiple energy storages in the vicinity, such as energy distribution or collection systems.

## AbstractEnergyBlock Class

Theis class serves as a base class for all energy-related blocks within the  JI Energy module. This abstract class extends the `Block` class and implements the  `BlockEntityProvider` interface, providing a foundation for blocks that require block entities to manage energy storage, transfer, or conversion. Subclasses of this class are expected to define specific behaviors and properties  related to energy management, such as energy capacity, input/output rates, and interaction with other energy systems. This class facilitates the integration of energy blocks into the Minecraft world by providing essential methods for block entity creation and management.

Key responsibilities of this class include:
- Providing a mechanism to create and manage block entities associated with energy blocks.
- Defining common properties and behaviors shared by all energy blocks.
- Facilitating interaction with other components of the JI Energy module and Minecraft's energy systems.

This class is already providing the needed `CODEC`, handling `getCodec`, `OnSyncedBlockEvent`, `getTicker`, `createScreenHandlerFactory`, `hasComparatorOutput`, and `getComparatorOutput` methods so that you can focus on your custom block without a need to override these methods

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the <strong>getTicker</strong> method is utilizing <strong>ITickBE</strong>'s <strong>createTicker</strong> method. Because of this, your block entity needs to implement <strong>ITickBE</strong> or <strong>ITickSyncBE</strong>. If you don't want to implement any of these methods, your block entity needs to override the getTicker method handling your own logic.
</div>

## AbstractEnergyBE Class

The abstract extension of `UpdatableEndTickBE` handling energy related logic.

---
---
> ##### ***`AddEnergyStorage(int capacity, int maxInsert, int maxExtract, Direction direction)`***

Adds a new energy storage to this block entity with the specified capacity, maximum insertion rate, maximum extraction rate, and direction. This method allows the block entity to manage energy storage by adding a new {@link SyncedEnergyStorage} instance to the internal {@link WrappedEnergyStorage}. The energy storage can be configured to accept energy from a specific direction or from all directions if no direction is specified.

Example usage:
```java
addEnergyStorage(10_000, 100, 100, Direction.NORTH);
```

---
---
> ##### ***`getEnergyStorage()`***

Retrieves the current energy storage associated with this block entity. This method allows other components of the system to access the energy storage configuration defined for this block entity. It returns an instance of `WrappedEnergyStorage` containing synchronized energy storage units, which can be used for operations related to energy management, such as checking current energy levels or manipulating energy within the storage.    

---
---
> ##### ***`readNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Deserializes the state of this block entity from an NBT compound. This method is responsible for retrieving and reconstructing the essential data of the block entity, including its energy storage, from the NBT format into memory. It extends the base behavior to ensure that the energy storage is correctly restored when the block entity is loaded or initialized.

---
---
> ##### ***`writeNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Serializes the state of this block entity to an NBT compound for storage or transmission. This method is responsible for converting essential data of the block entity, including its energy storage, into a format that can be saved in the game's NBT format. It extends the base behavior by adding the energy storage data for later retrieval.

---
---
> ##### ***`getScreenOpeningData(ServerPlayerEntity player)`***

Retrieves the data necessary for opening a screen on the client side. This method is used to provide the client with the position of the block entity when a screen associated with this block entity is opened. The position is encapsulated in a `BlockPosPayload` object, which is sent to the client to ensure that the correct block entity is referenced.

---
---
> ##### ***`getDisplayName()`***

Retrieves the display name of this block entity. This name is used to represent the block entity in the user interface, such as in the title of a screen or in tooltips. By default, this method returns `null`, indicating that no specific display name is set for this block entity. Subclasses can override this method to provide a custom display name that is more descriptive or context-specific, enhancing the user experience by providing meaningful information about the block entity.

---
---
> ##### ***`createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)`***

Creates a screen handler for the block entity, which is used to manage the interaction between the player's inventory and the block entity's GUI. This method is called when a player opens the block entity's screen, allowing for the synchronization of energy storage data between the client and server.

By default, this method returns null, indicating that no specific menu is created for this block entity. Subclasses can override this method to provide a custom screen handler that facilitates interaction with the block entity's inventory or other functionalities.

Example usage:
```java
@Override
public ScreenHandler createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)
{
    return new SomeScreenHandler(syncId, playerInventory, this);
}
```
