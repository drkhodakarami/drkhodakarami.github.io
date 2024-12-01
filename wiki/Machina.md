## Ji Machina

The [JiMachina](https://github.com/drkhodakarami/JiMachina) is the main abstraction layer for more advanced blocks that can have activation state, horizontal directional property, and handles comparator output automatically. Also, the library adds a new `AbstractAadvancedBE` block entity that handles all the wrapped containers we provide from other libraries (inventory, fluid and energy). This library provides 5 classes.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractAdvancedBE</strong>, extend your block from <strong>AbstractMachineBlock</strong>, implement and handle your custom methods for each one of these classes.
</div>

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the only abstract block class in this library that is capable of handling fluid item interaction with block, is <strong>AbstractFluidMachineBlock</strong>. If you need to extend your class from another base (other than <strong>AbstractFluidMachineBlock</strong> or <strong>AbstractFluidBlock</strong>), you need to add the code bellow to handle interaction between fluid item and the fluid contrainer for the block.
</div>

```java
protected ActionResult onUseWithItem(ItemStack stack, BlockState state, World world, BlockPos pos, PlayerEntity player, Hand hand, BlockHitResult hit)
{
    FluidHelper.interactWithBlock(world, pos, player, hand);
    return super.onUseWithItem(stack, state, world, pos, player, hand, hit);
}
```

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiMachina) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/machina/) repository for the project.

## AbstractMachineBase Class

The abstract class for machine blocks as a base, providing default functionality for having a block entity, handling the `codec`, `getTicker`, `onSyncedBlockEvent` and `createScreenHandlerFactory` methods. The class is extending from `AbstractHorizontalDirecitonBlock` meaning that it has a `FACING` property for horizontal directions. The `getTicker` method is utilizing the `ITickBE` interface meaning that your block entity that is associated to this block needs to implement the same interface or it's inherited child (`ITickSyncBE`). If you don't want to use these interfaces, you should override the `getTicker` method and implement your logic that can get help from validateTicker method provided in the class.

---
---
> ##### ***`validateTicker(BlockEntityType<A> givenType, BlockEntityType<E> expectedType, BlockEntityTicker<A> ticker)`***

A helper method that validates the ticker for the block entity. This method checks if the given type matches the expected type. Normally you don't need to override this method and you don't even need to call this method especially if your block entity is implementing either `ITickBE` or `ITickSyncBE`.

## AbstractActivatableMachineBlock Class

This class is extending `AbstractMachineBase` adding the `BooleanProperty` named `ACTIVATED`. This addition lets you have a custom variant for the model and at the same time handle the needed logic when the machine is activated or not. The class appends this property and you don't need to worry about that. The `onUse` method, by default cycles through the property when ever you use the block without any additonal check for the logic. You need to override this method and handle the logic properly. This default implementation is for demonstration and early development phase testing only.

## AbstractMachineBlock

The main abstract class for the machine blocks that extends from `AbstractActivatableMachineBlock` adding the comparator output handling logic only. Normally, when you need an advanced block associated to a block entity, you need to extend this block. Remember, as we said at the start of the page, this class does not provide any logic for handling fluid interaction. Read the section at the top of the page for more guidance about this.

Normally you shouldn't need to override the `hasComparatorOutput` and `getComparatorOutput`. The default implementation should be more than enough for most use cases.

## AbstractFluidMachineBlock

The main abstract class for the machine blocks that extends from `AbstractMachineBlock` adding the functionality for interaction between fluid items in hand and the fluid container in the block entity associated to the block itself. If your block entity does not have fluid storage, you should extend your block class from `AbstractMachineBlock` and not this one.

## AbstractAdvancedBE

Abstract class that represents an advanced block entity with functionalities like screen handler, ticking, synchronization of data, inventory, energy, and fluid management. This class extends from `UpdatableEndTickBE` meaning that not only it handles the GUI screen but also it handles the synchronization of data and deferres the update of the block entity to the end of the tick cycle. Also, this class implements the `ITickSyncBE` for handling the synchronization of data for different interanl storages (inventory, fluid and energy).

---
---
> ##### ***`addOutputInventory(int size, Direction direction)`***

Adds an output inventory to this block entity. This method initializes an output inventory of the specified size, allowing items to be extracted or transferred from the block entity in the specified direction. The output inventory facilitates item management and interactions with neighboring blocks or entities.

---
---
> ##### ***`AddEnergyStorage(int capacity, int maxInsert, int maxExtract, Direction direction)`***

Adds a new `SyncedEnergyStorage` to this block entity with the specified capacity, maximum insertion rate, maximum extraction rate, and direction. This method allows the block entity to manage energy storage by adding a new `SyncedEnergyStorage` instance to the internal `WrappedEnergyStorage`. The energy storage can be configured to accept energy from a specific direction or from all directions if no direction is specified (send null for direction for this perpouse).

---
---
> ##### ***`getEnergyStorage()`***

Retrieves the current energy storage associated with this block entity. This method allows other components of the system to access the energy storage configuration defined for this block entity. It returns an instance of `WrappedEnergyStorage` containing synchronized energy storage units, which can be used for operations related to energy management, such as checking current energy levels or manipulating energy within the storage.

---
---
> ##### ***`addFluidStorage(long capacity)`***

Adds a `SyncedFluidStorage` unit to this block entity. This method allows the block entity to manage fluid storage by specifying its maximum capacity. The fluid storage is used to hold and manipulate various fluids, facilitating interactions with surrounding blocks or entities that require fluid management.

---
---
> ##### ***`getFluidStorage()`***

Retrieves the weapped fluid storage instance associated with this block entity.

---
---
> ##### ***`getInventory()`***

Retrieves the wrapped inventory storage of this block entity. The inventory storage contains all inventories associated with the block entity and allows for various inventory operations.

---
---
> ##### ***`readNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Reads and initializes the state of this block entity from the specified NBT compound. This method extracts data stored in a given NBT (Named Binary Tag) compound and updates the block entity's attributes accordingly. It specifically handles the restoration of the inventory, energy storage, and fluid storage based on the data present in the NBT compound. The method should be called during world loading or when the block entity is created from persistent data.

---
---
> ##### ***`writeNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Writes the current state of this block entity to the specified NBT compound. This method serializes the block entity's inventory, energy storage, and fluid storage information into the provided NBT (Named Binary Tag) compound, allowing the state to be saved persistently. This is essential for ensuring that the block entity can be restored accurately when the world is reloaded or when the entity is retrieved from saved data.

---
---
> ##### ***`getDisplayName()`***

Retrieves the display name of this block entity. This name is used to represent the block entity in the user interface, such as in the title of a screen or in tooltips. By default, this method returns `null`, indicating that no specific display name is set for this block entity. Subclasses can override this method to provide a custom display name that is more descriptive or context-specific, enhancing the user experience by providing meaningful information about the block entity.

---
---
> ##### ***`createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)`***

Creates a screen handler for the block entity, which is used to manage the interaction between the player's inventory and the block entity's inventory. This method is called when a player opens the block entity's screen, allowing for the synchronization of inventory data between the client and server. By default, this method returns `null`, indicating that no specific menu is created for this block entity. Subclasses can override this method to provide a custom screen handler that facilitates interaction with the block entity's inventory or other functionalities.

Example usage:
```java
@Override
public ScreenHandler createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)
{
    return new SomeScreenHandler(syncId, playerInventory, this);
}
```
