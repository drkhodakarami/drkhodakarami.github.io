## JInventory

The [JInventory](https://github.com/drkhodakarami/JInventory) is the main abstraction layer for any block entity that will have inventories in themselves. This library has 8 classes and 3 interfaces.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractInventoryBE</strong> and if you need extra functionality on top of that, you can implement the IInventory interface. Also, you should take not of the <strong>OutputSlot</strong> and <strong>PredicateSlot</strong>.
</div>

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JInventory) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/inventory/) repository for the project.

## IInventory

The interface extends Inventory and can be used anytime you need an implementation for a block entity that handles simple inventories in themselves. This interface produce many different methods related to the inventory and how to handle slots. You can add this interface on top of any abstraction provided by this library to get more flexibility in your block entity.

Normally, if you want a simple inventory system, you can add a DefaultedList<ItemStack> in your block entity class and this list should behave as your inventory. You can use the code example as a starting point.

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
> ##### ***`getItems()`***

Retrieves a defaulted list of all the item stacks inside this inventory.

---
---
> ##### ***`of(DefaultedList<ItemStack> items)`***

Creates an inventory from the given defaulted list of item stacks.

---
---
> ##### ***`ofSize(int size)`***

Creates a new inventory with the given size and fills it with `empty` item stacks.

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

## IWrappedInventory

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

## IWrappedInventoryProvider

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

## RecipeSimpleInventory Class

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

## SyncingSimpleInventory Class

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

## AbstractInventoryBE Class

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