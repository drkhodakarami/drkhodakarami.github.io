## JInventory

The [JInventory](https://github.com/drkhodakarami/JInventory) is the main abstraction layer for any block entity that will have inventories in themselves. This library has 8 classes and 3 interfaces.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractInventoryBE</strong> and if you need extra functionality on top of that, you can implement the IInventory interface. Also, you should take not of the <strong>OutputSlot</strong> and <strong>PredicateSlot</strong>.
</div>

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JInventory) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/inventory/) repository for the project.

## IInventory

The interface extends Inventory and can be used anytime you need an implementation for a block entity that handles simple inventories in themselves. This interface produce many different methods related to the inventory and how to handle slots. You can add this interface on top of any abstraction provided by this library to get more flexibility in your block entity.

Normally, if you want a simple inventory system, you can add a `DefaultedList<ItemStack>` in your block entity class and this list should behave as your inventory. You can use the code example as a starting point.

Example Usage:
```java
public class SomeBlockEntity extends BlockEntity implements IInventory
{
    private final DefaultedList<ItemStack> inventory;
  
    public SomeBlockEntity(BlockEntityType type, BlockPos pos, BlockState state)
    {
        this.inventory = IInventory.ofSize(4);
    }
  
    @Override
    public DefaultedList<ItemStack> getItems()
    {
        return this.inventory;
    }
  
    @Override
    public void markDirty()
    {
        //Handle dirty state and what needs to be done here.
    }
}
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, if you are using anything other than defaulted list of item stacks for the invenotry, like the <strong>WrappedInventoryStorage</strong> provided by the library, you need to handle <strong>getItem</strong> and <strong>clear</strong> methods and based on what you have, you may even need to override other methods like <strong>getStack</strong>, <strong>removeStack</strong>, <strong>setStack</strong>, and <strong>markDirty</strong>. Also, keep in mind if you are using the wrapped inventory system from this library, you may not need this interface anymore because the wrapped inventory storage class should be more than enough to handle all of your needs. This interface is for small cases that you need a very simple inventory system.
</div>

---
---
> ##### ***`getItems()`***

Retrieves a defaulted list of all the item stacks inside this inventory.

---
---
> ##### ***`of(DefaultedList<ItemStack> items)`***

This static helper method creates an inventory from the given defaulted list of item stacks.

---
---
> ##### ***`ofSize(int size)`***

This static helper method creates a new inventory with the given size and fills it with `empty` item stacks.

---
---
> ##### ***`size()`***

Returns the size of the inventory.

---
---
> ##### ***`isEmpty()`***

Checks if the inventory is empty or not.

---
---
> ##### ***`getStack(int slot)`***

Retrieves the item stack in a slot.

---
---
> ##### ***`removeStack(int slot, int count)`***

Removes the given number of item stacks from the provided slot and returns the item stack that was removed from slot.

---
---
> ##### ***`removeStack(int slot)`***

Removes a full stack of the item stacks sitting inside the slot and returns the item stack that was removed from slot.

---
---
> ##### ***`setStack(int slot, ItemStack stack)`***

Replaces the current stack in an inventory slot with the provided item stack. If the slot previously had any stacks, they will be lost and it won't be recoverable. You should check if the stack has any item or not and if there is items in the slot, you should make sure that you can put the proper amount of the same item type in that stack unless you intentionally want to destroy previous items in the slot.

---
---
> ##### ***`clear()`***

This method clears the inventory. The internal mechanic is to call on the `getItems` method and clear that list. The default get item method is not implementing any code and it's up to you to handle the inventory list properly.

---
---
> ##### ***`markDirty()`***

Handles the dirty state of the block entity. The default implementation does nothing and if you need to handle anything, you should override this method.

---
---
> ##### ***`canPlayerUse(PlayerEntity player)`***

By default it returns true and can be overridden for any custom behavior check so that you can call this method and check if the inventory can be opened and used by player or not.

## IWrappedInventory

This is an interface representing a wrapped inventory that provides access to its underlying storage mechanism. This interface defines methods to retrieve the wrapped inventory, its storage, and the output inventory. For more information about wrapped inventory, check the `WrappedInventoryStorage` class.

---
---
> ##### ***`getInventory()`***

Retrieves the wrapped inventory storage. This method returns an instance of `WrappedInventoryStorage` that contains the wrapped inventories.

---
---
> ##### ***`getInventoryStorage(@Nullable Direction direction)`***

This method allows access to the inventory storage from a specific direction. If the direction is null, the default storage is returned. The inventory storage provides methods to interact with the inventory's contents and manage it's storage capabilities.

---
---
> ##### ***`getOutputInventory()`***

Retrieves the `SimpleInventory` assigned as the output inventory that is associated with this wrapped inventory. The output inventory is a specialized inventory used for output operations.

## IWrappedInventoryProvider

This interface extends the `IWrappedInventory` with addition methods.

If you want your block entity to use the `WrappedInventoryStorage` system, you can implement this interface and override the needed functionality. Alternatively, you can extend your block entity from `AbstractInventoryBE` that is already implementing this interface and adds the syncronization, update funtionality and GUI screen ability with the base implementation for using the wrapped inventory storage. Please read more about wrapped inventory from the class and look into abstract inventory information bellow.

---
---
> ##### ***`addOutputInventory(int size, Direction direction)`***

Adds an output inventory to this block entity. This method initializes an output inventory of the specific size, allowing items to be extracted or transferred from the block entity in the specified direction. The output inventory facilitates item management and interactions with neighboring blocks or entities.

---
---
> ##### ***`getInventory()`***

It's the same method as the parent `IWrappedInventory`.

---
---
> ##### ***`getInventoryStorage(@Nullable Direction direction)`***

It's the same method as the parent `IWrappedInventory`.

---
---
> ##### ***`getOutputInventory()`***

It's the same method as the parent `IWrappedInventory`.

## RecipeSimpleInventory Class

A specialized inventory class that extends `SimpleInventory` and implements `RecipeInput`. This class is designed to handle inventory operations specifically for recipes. It provides constructors to initialize the inventory with a specified size or with an array of `ItemStack` items.

---
---
> ##### ***`getStackInSlot(int slot)`***

Overrides the same method from `RecipeInput` interface to retrieve the `ItemStack` from a specified slot in the inventory.

## SyncingSimpleInventory Class

A specialized inventory class that extends `RecipeSimpleInventory` and syncronizes changes with an associated [NoScreenUpdatableBE](https://drkhodakarami.github.io/JiraLib#noscreenupdatablebe-class) (or any of its inherited) block entity. This class ensures that any modifications to the inventory are reflected in the block entity, allowing for seamless updates between the server and client. This class provides constructors to initialize the inventory with a specified size or with a predefined set of `ItemStack` items. It overrides the `markDirty` method to trigger an update on the associated block entity whenever the inventory is modified. Usage of this class is ideal in scenarios where inventory changes need to be syncronized with a block entity, ensuring that the client and server states remain consistent.

---
---
> ##### ***`sync()`***

The default implementation that will check if the block entity is marked dirty and the world is server side and then it checks if the block entity is inherited from [NoScreenUpdatableBE](https://drkhodakarami.github.io/JiraLib#noscreenupdatablebe-class) or not, if yes, it will call the `update` method and if not, it will call the `markDirty` on the block entity. Normally you don't need to override this method unless you want to change this functionality.

---
---
> ##### ***`markDirty()`***

This method will mark the inventory as dirty but will not call `markDirty` on the block entity. Because we have the `sync` method, we the call on the block entity will be deferred to the sync mehtod. Normally you don't need to override this method unless you want to change this functionality.

---
---
> ##### ***`blockEntity()`***

This method provides access to the block entity that is responsible for handling updates and syncronization between the server and client whenever the inventory changes.

## PredicateSimpleInventory Class

A specialized inventory class that extends `SyncingSimpleInventory` and incorporates a `BiPredicate` to validate items in specific slots. This class allows for custom validation logic to be applied to inventory slots, ensuring that only items meeting certain criteria can be placed in specific slots.
 
This class provides constructors to initialize the inventory with a specified size or with a predefined set of `ItemStack` items, along with a predicate for validation. It overrides the `isValid` method to utilize the predicate for determining the validity of items in slots.
 
Usage of this class is ideal in scenarios where inventory slots require specific validation logic, such as restricting certain items to specific slots based on custom rules.

---
---
> ##### ***`PredicateSimpleInventory(NoScreenUpdatableBE blockEntity, int size, BiPredicate<ItemStack, Integer> predicate)`***

Constructs a new instance of this class with the specified block entity, inventory size, and item validation predicate.

---
---
> ##### ***`PredicateSimpleInventory(NoScreenUpdatableBE blockEntity, BiPredicate<ItemStack, Integer> predicate, ItemStack... items)`***

Constructs a new instance of this class with the specified block entity, item validation predicate and initial set of items.

---
---
> ##### ***`isValid(int slot, ItemStack stack)`***

Determines whether the specified `ItemStack` is valid for the given slot index in the inventory or not utilizing the predefined `BiPredicate` to apply custom validation logic, ensuring that only items meeting certain criteria can be placed in specific slots.

---
---
> ##### ***`predicate()`***

Retrieves the `BiPredicate` used for validating items in specific slots of the inventory. This predicate allows for custom validation logic to be applied, ensuring that only items meeting certain criteria can be placed in specific slots.

## OutputSimpleInventory Class

A specialized inventory class that extends `PredicateSimpleInventory` and is designed specifically for output-only slots. This class ensures that no items can be placed into the inventory slots by using a `BiPredicate` that always returns `false`.

The `OutputSimpleInventory` class is useful in scenarios where inventory slots are intended solely for output purposes, preventing any manual insertion of items. It provides constructors to initialize the inventory with a specified size or with a predefined set of `ItemStack` items, while enforcing the output-only restriction.

Usage of this class is ideal in cases where inventory slots should only be populated programmatically, such as in automated systems or processing machines.

## PredicateSlot Class

A custom slot implementation that allows for conditional item insertion based on a specified predicate. This class extends the `Slot` class and provides additional functionality to control which items can be inserted into the slot. Two constructors are provided, one that accepts a custom predicate to determine if an item can be inserted, and another that defaults to using the `SimpleInventory.canInsert()` method as the predicate. Usage of this class allows for more flexible inventory management by enabling custom rules for item insertion.

Example Usage:
```java
//A slot that only accepts diamond
PredicateSlot slot = new PredicateSlot(this.inventory, 1, 50, 100, itemStack -> itemStack.isOf(Items.DIAMOND));
```

---
---
> ##### ***`PredicateSlot(Inventory inventory, int index, int x, int y, Predicate<ItemStack> predicate)`***

Constructs a new instance of the class with a specified inventory, slot index, position in the GUI and item insertion predicate.

---
---
> ##### ***`PredicateSlot(SimpleInventory inventory, int index, int x, int y)`***

Constructs a new instance of the class with a specified inventory, slot index and position in the GUI. For predicate, the constructor is using `SimpleInventory.canInsert()`.

---
---
> ##### ***`canInsert(ItemStack stack)`***

Determines whether the specified `ItemStack` can be inserted into this slot. This method uses the predicate defined for this slot to evaluate if the given item stack is allowed to be placed in the slot. The predicate provides custom logic for item insertion, allowing for flexible inventory management.

## TaggedSlot Class

A specialized slot implementation that extends `PredicateSlot` to create a slot that accepts items that are assigned to a tag key. This class is designed to make sure only selected items can insert the slot. This makes it suitable for use in scenarios where items should only be added in certain slot, such as crafting or processing inputs. This example creates a tagged slot at index 0 that only accepts items which has been assinged to SOME_ITEM_TAG.
  
Example usage:
```java
Inventory inventory = new SimpleInventory(9);
TaggedSlot taggedSlot = new TaggedSlot(inventory, 0, 10, 30, SOME_ITEM_TAG);
```

---
---
> ##### ***`TaggedSlot(Inventory inventory, int index, int x, int y, TagKey<Item> tagKey)`***

Constructs an instance of the class with the specified inventory, slot index, and position in the GUI, and the predicate tag key.

## OutputSlot Class

A specialized slot implementation that extends `PredicateSlot` to create an output-only slot. This class is designed to prevent any item from being inserted into the slot by using a predicate that always returns `false`. This makes it suitable for use in scenarios where items should only be extracted from the slot, such as crafting or processing outputs. This example creates an output slot at index 0 that does not allow any item insertion.
  
Example usage:
```java
Inventory inventory = new SimpleInventory(9);
OutputSlot outputSlot = new OutputSlot(inventory, 0, 10, 30);
```

---
---
> ##### ***`OutputSlot(Inventory inventory, int index, int x, int y)`***

Constructs an instance of the class with the specified inventory, slot index, and position in the GUI. This constructor initializes an output-only slot that does not allow any item insertion by using `itemStack -> false` as the predicate.

## WrappedInventoryStorage Class

The wrapped inventory is a collection of multiple inventories that can be accessed and managed collectively. It supports operations such as retrieving the inventory storage for sprecific direction and accessing the output inventory. Wrapped inventory provides the needed flexibility in the types of inventories that can be wrapped and managed.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the current implementation, version 1.1.2+MC_1.21.3, does not provide default behavior for handling the <strong>FACING</strong> of the block to retrieve proper inventory based on the rotation and by default it assumes that the block is facing north. If you want to handle the rotational facing, you need to add your own logic (until we handle it properly ourselves in the library).
</div>

---
---
> ##### ***`addInventory(@NotNull SimpleInventory inventory)`***

Creates a new wrapped inventory storage with the given inventories. The inventory will be assigned to no directions because we didn't set a specific direction for the inventory.

---
---
> ##### ***`addInventory(@NotNull SimpleInventory inventory, Direction direction)`***

Creates a new wrapped inventory storage with the given inventory and the direction.

---
---
> ##### ***`addSyncInventory(NoScreenUpdatableBE blockEntity, int size)`***

Creates a new wrapped inventory storage with a new synced inventory of the given size. The inventory will be assigned to no directions because we didn't set a specific direction for the inventory.

---
---
> ##### ***`addSyncInventory(NoScreenUpdatableBE blockEntity, int size, Direction direction)`***

Creates a new wrapped inventory storage with a new synced inventory of the given size and direction.

---
---
> ##### ***`getInventories()`***

Retrieves the list of inventories wrapped by this storage. Each inventory in the list is an instance of `SimpleInventory`. This mehtod provides access to the underlying inventories, allowing for direct interaction with each inventory's content.

---
---
> ##### ***`getStorages()`***

Retrieves the list of `InventoryStorage` instances corresponding to each inventory in the inventories list. This list is used to handle the storage operations for each inventory, allowing for individual management of each inventory's storage capabilities.

---
---
> ##### ***`getSidedStorageMap()`***

Retrieves the map that associates each `Direction` with its corresponding `InventoryStorage`. This map is used to manage and access inventories that are accessible from specific directions, allowing for direction-based inventory operations.

---
---
> ##### ***`getCombinedStorage()`***

Retrieves the combined storage that aggregates all `InventoryStorage` instances in this wrapper. This combined storage allows for unified operations across all wrapped inventories, providing a single interface to interact with multiple inventories as if they were one.

---
---
> ##### ***`getSlot(int slot, Direction direction)`***

Retrieves the storage slot at the specific index for the given direction.

---
---
> ##### ***`getSlots(Direction direction)`***

Retrieves all storage slots for the given direction.

---
---
> ##### ***`getStorage(Direction direction)`***

Retrieves the `InventoryStorage` for the given direction.

---
---
> ##### ***`getInventory(int index)`***

Retrieves the `SimpleInventory` for the given index.

---
---
> ##### ***`getStacks()`***

Retrieves a list of `ItemStack` instances representing the contents of all inventories wrapped by this storage. This method provides a unified view of all item stacks across the multiple inventories allowing for easy access and manipulation of the itemns stored within.

---
---
> ##### ***`getStacks(int index)`***

Retrieves a list of `ItemStack` instances representing the contents of the inventory at the specified index. This method provides access to the item stacks within a specified inventory, allowing for easy manipulation and retrieval of items stored in that particular inventory.

---
---
> ##### ***`getRecipeInventory()`***

Retrieves a `RecipeSimpleInventory` instance that represents the inventory used for crafting recipes. This method provides access to a specialized inventory designed to interact with crafting mechanics, allowing for retrieval and manipulation of items specifically for recipe processing.

---
---
> ##### ***`checkSize(int size)`***

Validates that the specified size is appropriate for the inventory. This method ensures that the inventory can be accommodate the given number of slots. This is typically used to enforce constraints on inventory size during initialization or resizing operations.

---
---
> ##### ***`onOpen(@NotNull PlayerEntity player)`***

Handles the event when a player opens the inventory. This method is typically used to perform actions such as updating the inventory state, notifying listeners, or triggering specific behaviors when a player interacts with the inventory. It ensures that any necessary setup or state changes occure when the inventory is accessed by a player.

---
---
> ##### ***`onClose(@NotNull PlayerEntity player)`***

Handles the event when a player closes the inventory. This method is typically used to perform actions such as saving the inventory state, notifying listeners, or triggering specific behaviors when a player stops interacting with the inventory. It ensures that any necessary cleanup or state updates occure when the inventory is closed by a player.

---
---
> ##### ***`dropContents(@NotNull World world, @NotNull BlockPos pos)`***

Drops the contents of the inventory into the world at the specified position. This method is typically used when an inventory block is broken or needs to release its items into the world. It ensures that all items contained within the inventory are scattered around the given position in the world.

---
---
> ##### ***`addDefaultOutputInventory(NoScreenUpdatableBE blockEntity, int size, Direction direction)`***

Adds a default output inventory to the specified [NoScreenUpdatableBE](https://drkhodakarami.github.io/JiraLib#noscreenupdatablebe-class) block entity. This method initializes an inventory with the given size and associates it with a specific direction. It is typically used to set up the output storage for a block entity, allowing items to be output in a specified direction.

---
---
> ##### ***`writeNbt(RegistryWrapper.WrapperLookup registryLookup)`***

Serializes the inventory data into an `NbtList` using the provided `RegistryWrapper.WrapperLookup`. This method is used to convert the current state of the inventory into `NBT` format, which can be stored or transferred as needed. The serialization process involves writing each inventory's contents into the NBT structure.

---
---
> ##### ***`readNbt(NbtList nbt, RegistryWrapper.WrapperLookup registryLookup)`***

Deserializes the inventory data from the provided `NbtList` using the given `RegistryWrapper.WrapperLookup`. This method is used to restore the state of the inventory from `NBT` format, allowing the inventory to be reconstructed with its previous contents. The deserialization process involves reading each inventory's contents from the NBT structure.

## AbstractInventoryBE Class

Abstract class that represents a block entity with funtionalities like a screen handler, ticking, updating, and syncronizing data, that contains a wrapped inventory storage.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, version 1.1.2+MC_1.21.3 is extending from <strong>NOScreenUpdatableBE</strong> and implemets the <strong>ITickSync</strong> and <strong>ExtendedScreenHandlerFactory</strong> by itself, meaning that you need to handle the deferred behavior for the end tick update manually. <p/><p/>In the next versions this will change and it will extend <strong>UpdateEndTickBE</strong> because normally when you have inventories, you want to have screens and by implementing <strong>ITickSyncBE</strong> we want to syncronize these inventories so the update should automatically deferred to the end of the tick cycle.
</div>

---
---
> ##### ***`addOutputInventory(int size, Direction direction)`***

Adds an output inventory to this block entity. This method initializes an output inventory of the specified size, allowing items to be extracted or transferred from the block entity in the specified direction. The output invventory facilitates item management and interactions with neighboring blocks or entities.

---
---
> ##### ***`getInventory()`***

Retrieves the wrapped inventory storage of this block entity. The inventory storage contains all inventories associated with the block entity and allows for various inventory operations.

---
---
> ##### ***`getInventoryStorage(@Nullable Direction direction)`***

Retrieves the storage for the inventory associated with this block entity in the specified direction. This method allows for accessing the inventory storage in relation to the block entity's orientation, enabling proper interactions with neighboring blocks or entities.

---
---
> ##### ***`getOutputInventory()`***

Retrieves the output inventory of this block entity. The output inventory is specifically designed for holding items that can be extracted or tranderred from the block entity. If the inventory has been initialized and contains output storage, this method returns the corresponding simple inventory, otherwise, it returns null.

---
---
> ##### ***`readNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Deserializes the state of this block entity from an NBT compound. This method is responsible for retrieving and reconstructing the essential data of the block entity, including its inventory, from the NBT format into memory. It extends the base behavior to ensure that the inventory is correctly restored when the block entity is loaded or initialized.

---
---
> ##### ***`writeNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Serializes the state of this block entity to an NBT compound for storage or transmission. This method is responsible for converting essential data of the block entity, including its inventory, into a format that can be saved in the game's NBT format. It extends the base behavior by adding the inventory data for later retrieval.

---
---
> ##### ***`getScreenOpeningData(ServerPlayerEntity player)`***

Retrieves the data necessary for opening a screen on the client side. This method is used to provide the client with this block entity is opened. The position is encapsulated in a [BlockPosPayload](https://drkhodakarami.github.io/JiraLib#blockpos-payload) object, which is sent to the client to ensure that the correct block entity is referenced.

---
---
> ##### ***`getDisplayName()`***

Retrieves the display name of this block entity. This name is used to represent the block entity in the user interface, such as in the title of a screen or in tooltips. By default, this method returns null, indicating that no specific display name is set for this block entity.
     
Subclasses can override this method to provide a custom display name that is more descriptive or context-specific, enhancing the user experience by providing meaningful information about the block entity.

---
---
> ##### ***`createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)`***

Creates a screen handler for the block entity, which is used to manage the interaction between the player's inventory and the block entity's inventory. This method is called when a player opens the block entity's screen, allowing for the synchronization of inventory data between the client and server.

By default, this method returns null, indicating that no specific menu is created for this block entity. Subclasses can override this method to provide a custom screen handler that facilitates interaction with the block entity's inventory or other functionalities.

Example usage:
```java
@Override
public ScreenHandler createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player) 
{
    return new SomeScreenHandler(syncId, playerInventory, this);
}
```
