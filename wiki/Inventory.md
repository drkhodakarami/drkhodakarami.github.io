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

This interface extends the `IWrappedInventory` with addition information, methods and default constant values for the output inventory. The default values for the output inventory that his interface provides are:

- DEFAULT_OUTPUT_INDEX = 0
- DEFAULT_OUTPUT_SIZE = 1
- DEFAULT_OUTPUT_DIRECTION = Direction.DOWN

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

A specialized inventory class that extends `RecipeSimpleInventory` and syncronizes changes with an associated `NoScreenUpdatableBE` (or any of its inherited) block entity. This class ensures that any modifications to the inventory are reflected in the block entity, allowing for seamless updates between the server and client. This class provides constructors to initialize the inventory with a specified size or with a predefined set of `ItemStack` items. It overrides the `markDirty` method to trigger an update on the associated block entity whenever the inventory is modified. Usage of this class is ideal in scenarios where inventory changes need to be syncronized with a block entity, ensuring that the client and server states remain consistent.

---
---
> ##### ***`sync()`***

The default implementation that will check if the block entity is marked dirty and the world is server side and then it checks if the block entity is inherited from `NoScreenUpdatableBE` or not, if yes, it will call the `update` method and if not, it will call the `markDirty` on the block entity. Normally you don't need to override this method unless you want to change this functionality.

---
---
> ##### ***`markDirty()`***

This method will mark the inventory as dirty but will not call `markDirty` on the block entity. Because we have the `sync` method, we the call on the block entity will be deferred to the sync mehtod. Normally you don't need to override this method unless you want to change this functionality.

---
---
> ##### ***`blockEntity()`***

This method provides access to the block entity that is responsible for handling updates and syncronization between the server and client whenever the inventory changes.

## PredicateSimpleInventory Class

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## OutputSimpleInventory Class

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## PredicateSlot Class

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## OutputSlot Class

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## WrappedInventoryStorage Class

The wrapped inventory is a collection of multiple inventories that can be accessed and managed collectively. It supports operations such as retrieving the inventory storage for sprecific direction and accessing the output inventory. Wrapped inventory provides the needed flexibility in the types of inventories that can be wrapped and managed.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the current implementation, version 1.1.2+MC_1.21.3, does not provide default behavior for handling the <strong>FACING</strong> of the block to retrieve proper inventory based on the rotation and by default it assumes that the block is facing north. If you want to handle the rotational facing, you need to add your own logic (until we handle it properly ourselves in the library).
</div>

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text

## AbstractInventoryBE Class

text

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, version 1.1.2+MC_1.21.3 is extending from <strong>NOScreenUpdatableBE</strong> and implemets the <strong>ITickSync</strong> and <strong>ExtendedScreenHandlerFactory</strong> by itself, meaning that you need to handle the deferred behavior for the end tick update manually. <p/><p/>In the next versions this will change and it will extend <strong>UpdateEndTickBE</strong> because normally when you have inventories, you want to have screens and by implementing <strong>ITickSyncBE</strong> we want to syncronize these inventories so the update should automatically deferred to the end of the tick cycle.
</div>

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

---
---
> ##### ***``***

text

---
---
> ##### ***``***

text