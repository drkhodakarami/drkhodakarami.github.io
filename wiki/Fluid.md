## Ji Fluid

The [JiFluid](https://github.com/drkhodakarami/JiFluid) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle fluid containers. It provides abstract classes, wrapped fluid containers, and helper methods that will handle more complex fluid transfer for you. It contains 6 classes, 1 interfaces, and 1 enum. The [JiMachina](https://github.com/drkhodakarami/JiMachina) library is depending on this one.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractFluidBE</strong>, extend your block from <strong>AbstractFluidBlock</strong> and use helper methods provided by FluidHelper class.
</div>

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiFluid) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/fluid/) repository for the project.

## IWrappedFluidProvider

Interface for providing wrapped fluid storage capabilities. This interface defines methods and constants for managing fluid storage within a wrapped context. Implementations of this interface can add fluid storages and retrieve the wrapped fluid storage instance.

---
---
> ##### ***`addFluidStorage(long capacity)`***

Adds a new fluid storage with the specified capacity.

---
---
> ##### ***`WrappedFluidStorage getFluidStorage()`***

Retrieves the `WrappedFluidStorage` instance.

## FluidTooltipMode

Enum representing the different modes for displaying fluid tooltips. This can be used to customize how fluid quantities are shown in the UI.

The possible cases are:
- SHOW_AMOUNT_FABRIC: Shows value based on fabric's default `droplet` system.
- SHOW_AMOUNT_AND_CAPACITY_FABRIC: Shows value/capacity based on fabric's default `droplet` system.
- SHOW_AMOUNT_MB: Shows value based on the milli-bucket system (each bucket equals 1000 mB)
- SHOW_AMOUNT_AND_CAPACITY_MB: Shows value/capacity based on the milli-bucket system.

## FluidStackRenderer Class

---
---
> ##### ***`FluidStackRenderer()`***

This constructor will create an intance of the class with one Bucket as the maximum capacity of the fluid stack, `true` as the flag to show the capacity and use milli-buckets, and 16 pixels as default for width and height values.

---
---
> ##### ***`FluidStackRenderer(long capacity, boolean showCapacity, int width, int height)`***

Constructs a `FluidStackRenderer` with specified capacity, tooltip display options, and UI dimensions. This constructor allows for customization of the fluid rendering by specifying the maximum capacity of the fluid stack, whether the capacity should be displayed in the tooltip, and the dimensions of the rendered fluid stack in the user interface. This constructor forces the fluid stack to use milli-bucket system.

The `capacity` parameter defines the maximum amount of fluid stack, that the renderer can display. This is crucial for calculating the proportion of the fluid to render visually and for displaying accurate information in the tooltip.

The `showCapacity` parameter determines whether the tooltip should include the fluid's capacity alongside its current amount. This provides users with a clearer understanding of how much fluid is present relative to the maximum capacity.

The `width` and `height` parameters specify the dimensions of the fluid stack's visual representation in pixels. These dimensions allow for flexibility in how the fluid is displayed, accommodating different UI layouts and design requirements.

---
---
> ##### ***`FluidStackRenderer(long capacity, boolean showCapacity, boolean useMilliBuckets, int width, int height)`***

Constructs a `FluidStackRenderer` with specified capacity, tooltip display options, unit preference, and dimensions. This constructor provides extensive customization for rendering fluid stacks by allowing the specification of the fluid's maximum capacity, tooltip display preferences, unit of measurement, and the dimensions of the rendered fluid stack in the user interface.    

---
---
> ##### ***`FluidStackRenderer(long capacity, FluidTooltipMode tooltipMode, int width, int height)`***

Constructs a `FluidStackRenderer` with specified capacity, tooltip mode, and dimensions. This constructor provides detailed customization for rendering fluid stacks by allowing the specification of the fluid's maximum capacity, the mode for displaying tooltips, and the dimensions of the rendered fluid stack in the user interface.
     
The `tooltipMode` parameter, defined by the `FluidTooltipMode` enum, specifies how the fluid's amount and capacity should be displayed in the tooltip. This allows for flexibility in presenting fluid information according to user preferences or application requirements. The available modes include displaying amounts in both Fabric conventions and milli-buckets, with options to include or exclude capacity information.</p>

---
---
> ##### ***`getCapacity()`***

Retrieves the maximum capacity of the fluid stack. This method returns the capacity value that was set during the instantiation of the `FluidStackRenderer`. The capacity is used to determine the proportion of the fluid that should be rendered and displayed in tooltips. The capacity is a crucial parameter for rendering fluid stacks, as it defines the upper limit of fluid that can be visually represented. This allows for accurate scaling of the fluid's visual representation and ensures that the tooltip information is consistent with the actual fluid content. By providing access to the capacity value, this method enables other components or systems to query and utilize the fluid stack's capacity for various purposes, such as calculations, comparisons, or display adjustments.

---
---
> ##### ***`setZOrder(int index)`***

Sets the z-order for rendering the fluid stack, which affects the rendering depth. The z-order determines the stacking order of rendered elements, with higher values indicating that the fluid stack should be rendered on top of other elements with lower z-order values. Adjusting the z-order is useful in complex user interfaces where multiple elements are rendered in overlapping layers. By setting the z-order, you can control which elements appear in front of or behind others, ensuring that the fluid stack is displayed correctly within the intended visual hierarchy.

---
---
> ##### ***`drawFluid(DrawContext context, FluidStack fluid, int x, int y, int width, int height, long maxCapacity)`***

Renders the specified `FluidStack` at a given position and with specified dimensions. This method is responsible for visually representing the fluid stack within the user interface, taking into account the fluid's current amount relative to its maximum capacity. The rendering process involves setting the appropriate texture and color for the fluid, and drawing the fluid's sprite in a vertical manner to accurately depict the fluid level. The method ensures that the fluid is rendered with the correct proportions and visual style, adhering to the game's graphical standards. The `context` parameter provides the drawing context, which includes the necessary rendering tools and settings. The `fluid` parameter specifies the `FluidStack` to be rendered, containing both the fluid type and its current amount. The `x` and `y` parameters define the top-left corner of the rendering area, while the `width` and `height` parameters specify the dimensions of the rendered fluid stack in pixels. These dimensions allow for flexibility in how the fluid is displayed, accommodating different UI layouts and design requirements. The `maxCapacity` parameter represents the maximum capacity of the fluid stack, and is used to calculate the proportion of the fluid to render. This ensures that the visual representation accurately reflects the fluid's current state.

---
---
> ##### ***`getTooltip(FluidStack fluidStack, Item.TooltipContext tooltipFlag, String modid)`***

Generates a tooltip for the specified `FluidStack`, providing detailed information about the fluid's type, amount, and optionally its capacity, formatted according to the current tooltip mode. This method constructs a list of `Text` components that represent the tooltip for a given `FluidStack`. The tooltip includes the fluid's display name and its amount, with the option to also display the capacity based on the `FluidTooltipMode` configuration. The tooltip is designed to be informative and user-friendly, aiding players in understanding the fluid's properties at a glance.

The `fluidStack` parameter specifies the `FluidStack` for which the tooltip is generated, containing both the fluid type and its current amount. The `tooltipFlag` parameter provides context for the tooltip, such as whether advanced tooltips are enabled. The `modid` parameter is used to localize the tooltip text, ensuring that it is consistent with the mod's language settings.

---
---
> ##### ***`getWidth()`***

Retrieves the width of the rendered fluid stack in pixels. This method returns the width dimension that was set during the instantiation of the `FluidStackRenderer`. The width is a critical parameter for determining how the fluid stack is visually represented within the user interface. The width value is used to define the horizontal size of the fluid's visual representation, ensuring that it fits appropriately within the designated UI space. This allows for consistent and accurate rendering of the fluid stack, aligning with the overall design and layout of the interface. By providing access to the width value, this method enables other components or systems to query and utilize the fluid stack's width for various purposes, such as layout calculations, rendering adjustments, or collision detection.

---
---
> ##### ***`getHeight()`***

Retrieves the height of the rendered fluid stack in pixels. This method returns the height dimension that was set during the instantiation of the `FluidStackRenderer`. The height is a crucial parameter for determining how the fluid stack is visually represented within the user interface. The height value is used to define the vertical size of the fluid's visual representation, ensuring that it fits appropriately within the designated UI space. This allows for consistent and accurate rendering of the fluid stack, aligning with the overall design and layout of the interface. By providing access to the height value, this method enables other components or systems to query and utilize the fluid stack's height for various purposes, such as layout calculations, rendering adjustments, or collision detection.

## SyncedFluidStorage Class

Represents a synchronized fluid storage system that is associated with a specific block entity. This class extends the functionality of `SingleFluidStorage` to manage fluid storage with a defined capacity while ensuring synchronization with the associated block entity. The fluid storage has a specified capacity, and upon committing changes to the storage, it informs the associated block entity to update its state. If the block entity implements the `NoScreenUpdatableBE` interface, it will call the update method; otherwise, it will mark the block entity as dirty to prompt a save.

Example Usage:
```java
UpdatableBE blockEntity = ...; // your block entity instance
long capacity = FluidConstants.BUCKET;
SyncedFluidStorage storage = new SyncedFluidStorage(blockEntity, capacity);
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, this class is implementing ISync interface and should syncronize the storage. The default implementation should be more than enough for most use cases. If you don't need to change the default behavior, you don't need to override the sync method.
</div>

---
---
> ##### ***`getCapacity(FluidVariant fluidVariant)`***

Returns the maximum capacity of the fluid storage for the sepcified fluid variant.

---
---
> ##### ***`onFinalCommit()`***

This method is being called when the final commit has been made to the fluid storage. This method overrides the default implemntation in `SingleFluidStorage` to provide additional behavior. When this method is called, it will mark the storage as dirty. This flag will be checked later when the sync method is called to syncronize and update the container. Normally you shouldn't find a need to override this method. If you need to override this method, remember to call the super to make sure that this functionality remains intact.

---
---
> ##### ***`sync()`***

This method will syncronize the fluid storage with the client and calls the `update` method associated to block entity. If the block entity is not an instance of `NoScreenUpdatableBE`, then the block entity will simply call the `markDirty` method of itself.

---
---
> ##### ***`getBlockEntity()`***

Retrieves the block entity associated with this fluid storage.

## WrappedFluidStorage Class

A wrapper class for managing multiple `SingleFluidStorage` instances. This class provides method to add, retrieve, and manage fluid storages associated with different directions or indices.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the 1.1.2+MC_1.21.3 version, does not provide default behavior for handling the <strong>FACING</strong> of the block to retrieve proper storage based on the rotation and by default it assumes that the block is facing north. If you want to handle the rotational facing, you need to add your own logic. From 1.1.3+MC_1.21.3 and forward, the library is handling the FACING direction (look into <strong>AbstractFluidBE</strong>, the <strong>getProvider</strong> method is getting the directional relative storage for that block entity).
</div>

---
---
> ##### ***`addStorage(SingleFluidStorage storage)`***

Adds a storage to the list without associating it with a specific direction.

---
---
> ##### ***`addStorage(SingleFluidStorage storage, Direction direction)`***

Adds a storage to the list and associates it with a specific direction.

---
---
> ##### ***`addSyncedStorage(NoScreenUpdatableBE blockEntity, long capacity)`***

Adds a synced storage associated with a block entity and capacity.

---
---
> ##### ***`addSyncedStorage(NoScreenUpdatableBE blockEntity, long capacity, Direction direction)`***

Adds a synced storage associated with a block entity, capacity, and direction.

---
---
> ##### ***`getStorages()`***

Retrieves the list of all storages.

---
---
> ##### ***`getSidedStorageMap()`***

Retrieves the map of storages associated with directions.

---
---
> ##### ***`getAmount(Direction direction)`***

Retrives the amount of fluid in the storage associated with a direction.

---
---
> ##### ***`getCapacity(Direction direction)`***

Retrieves the capacity of the storage associated with a direction.

---
---
> ##### ***`getType(Direction direction)`***

Retrieves the fluid type in the storage associated with a direction.

---
---
> ##### ***`getAmount(int index)`***

Retrieves the amount of fluid in the storage at a specific index.

---
---
> ##### ***`getCapacity(int index)`***

Retrieves the capacity of the storage at a specific index.

---
---
> ##### ***`getType(int index)`***

Retrieves the fluid type in the storage at a specific index.

---
---
> ##### ***`getStorage(Direction direction)`***

Retrieves the storage associated with a direction.

---
---
> ##### ***`getStorage(int index)`***

Retrieves the storage at a specific index.

---
---
> ##### ***`getFluids()`***

Retrieves a list of fluid stacks representing the fluids in all storages.

---
---
> ##### ***`writeNbt(RegistryWrapper.WrapperLookup registryLookup)`***

Serializes the fluid storages to an NBT list.

---
---
> ##### ***`readNbt(NbtList nbt, RegistryWrapper.WrapperLookup registryLookup)`***

Deserializes the fluid storages from an NBT list.

---
---
> ##### ***`getProvider(Direction direction, Direction facing)`***

Retrieves a `SingleFluidStorage` related to a direction using the facing of the block.

Example Usage:
```java
WrappedFluidStorage fluidStorage =...; //Creating a fluid storage
SingleFluidStorage tank = fluidStorage.getProvider(Direction.NORTH, getCachedState().get(SomeBlock.FACING));
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the <strong>direction</strong> that you send in to retrieve the storage, should be the same direction that you assigned when creating the storage and adding it to the wrapped storage. In other words, if the tank should be on the east side of the block because of it's rotation in the world, you should completely ignore this fact and simply use the same direction that you used to register the storage into the wrapped container (in this example we had originally used north). The easy way to understand what needs to be done, is to put the block in such a way that <strong>FACING</strong> property will return <strong>north</strong>. Then consider what should be on each side of the container and register the storages with the proper direction at that placed configuration. From that point forward, you can assume that the whole system will work even when you rotate the block by facing property.
</div>

## FluidHelper Class

This class provides utility methods for handling fluid transfers and interactions within a Minecraft mod environment. It includes methods for transferring fluids between tanks and inventories, interacting with fluid storage blocks, and converting fluid measurements. This class utilizes the Fabric API for fluid handling and storage.

---
---
> ##### ***`handleTankTransfer(World world, BlockPos pos, SingleVariantStorage<FluidVariant> tank, Inventory inputInventory, Inventory outputInventory, int inputSlot, int outputSlot)`***

Facilitates the bidirectional transfer of fluids between two inventories and a fluid storage tank. This method first attempts to transfer fluid from the specified input inventory slot to the tank. If this transfer is not possible, it then attempts to transfer fluid from the tank to the specified output inventory slot. The method ensures that the transfer only occurs if there is sufficient fluid in the source and adequate space in the destination.

---
---
> ##### ***`transferFromTank(World world, BlockPos pos, SingleVariantStorage<FluidVariant> tank, Inventory inputInventory, Inventory outputInventory, int inputSlot, int outputSlot)`***

Transfers fluid from a fluid storage tank to a specified output slot in an inventory. This method checks if the output slot in the inventory can receive bucket items and if the tank has sufficient fluid to transfer. If both conditions are met, the fluid is transferred from the tank to the inventory. The method also handles playing the appropriate sound effects during the transfer.

---
---
> ##### ***`transferToTank(World world, BlockPos pos, SingleVariantStorage<FluidVariant> tank, Inventory inputInventory, Inventory outputInventory, int inputSlot, int outputSlot)`***

Transfers fluid from a specified input slot in an inventory to a fluid storage tank. This method checks if the input slot in the inventory contains full bucket items that can be transferred to the tank and if the tank has enough capacity to receive the fluid. If both conditions are met, the fluid is transferred from the inventory to the tank. The method also handles playing the appropriate sound effects during the transfer.

---
---
> ##### ***`interactWithBlock(World world, BlockPos pos, PlayerEntity player, Hand hand)`***

Allows a player to interact with a fluid storage block in the game world. This method checks each side of the block for a fluid storage capability and attempts to interact with it using the player's current item in hand. If a fluid storage is found, the method facilitates the interaction, which may involve transferring fluid between the player's item and the block's storage. The method also handles any necessary sound effects during the interaction.

---
---
> ##### ***`isTankEmpty(SingleVariantStorage<FluidVariant> tank)`***

Determines whether a given fluid storage tank is empty. This method checks the amount of fluid currently stored in the tank and returns true if the tank contains no fluid. It is useful for validating whether a tank can receive more fluid or if it needs to be refilled.

---
---
> ##### ***`isTankEmpty(Storage<FluidVariant> tank)`***

Determines whether a given fluid storage tank is empty. This method checks the amount of fluid currently stored in the tank and returns true if the tank contains no fluid. It is useful for validating whether a tank can receive more fluid or if it needs to be refilled.

---
---
> ##### ***`isOutputReceivable(Inventory inventory, int slot, boolean shouldAcceptEmpty, FluidVariant variant)`***

Determines whether a specified slot in an inventory can receive additional fluid items. This method checks the current contents of the slot and evaluates whether it can accept more items based on the specified fluid variant and conditions. It is useful for validating if a slot is ready to receive fluid items during a transfer operation.

---
---
> ##### ***`isEmptyBucket(Inventory inventory, int slot)`***

Checks whether a specified slot in an inventory contains an empty bucket. This method evaluates the item present in the given slot and determines if it is an empty bucket item. It is useful for operations that require identifying empty buckets for fluid transfer or crafting purposes.

---
---
> ##### ***`simulateInsertion(Storage<FluidVariant> storage, FluidVariant resource, long amount, Transaction outer)`***

Simulates the insertion of a specified amount of fluid type into a `FluidStorage`. This method opens a nested transaction to determine the maximum amount of fluid that can be inserted without actually committing the transaction.

---
---
> ##### ***`simulateInsertion(SingleVariantStorage<FluidVariant> storage, FluidVariant resource, long amount, Transaction outer)`***

Simulates the insertion of a specified amount of fluid type into a `FluidStorage`. This method opens a nested transaction to determine the maximum amount of fluid that can be inserted without actually committing the transaction.

---
---
> ##### ***`sameFluidInTank(Inventory inventory, int slot, SingleVariantStorage<FluidVariant> tank)`***

Checks whether the fluid contained in a specified slot of an inventory is the same as the fluid stored in a given fluid storage tank. This method compares the fluid variant in the inventory slot with the fluid variant in the tank to determine if they match. It is useful for operations that require verifying fluid consistency between an inventory and a tank, such as ensuring compatibility before transferring fluids.

---
---
> ##### ***`sameFluidInTank(Inventory inventory, int slot, Storage<FluidVariant> tank)`***

Checks whether the fluid contained in a specified slot of an inventory is the same as the fluid stored in a given fluid storage tank. This method compares the fluid variant in the inventory slot with the fluid variant in the tank to determine if they match. It is useful for operations that require verifying fluid consistency between an inventory and a tank, such as ensuring compatibility before transferring fluids.

---
---
> ##### ***`isTankFull(Inventory inventory, int slot, SingleVariantStorage<FluidVariant> tank)`***

Determines whether a specified fluid storage tank is full based on the fluid contained in a given slot of an inventory. This method checks if the fluid variant in the inventory slot matches the fluid variant in the tank and if the tank has reached its maximum capacity for that fluid. It is useful for operations that require verifying if a tank can no longer accept additional fluid of the same type.

---
---
> ##### ***`isTankFull(Inventory inventory, int slot, Storage<FluidVariant> tank)`***

Determines whether a specified fluid storage tank is full based on the fluid contained in a given slot of an inventory. This method checks if the fluid variant in the inventory slot matches the fluid variant in the tank and if the tank has reached its maximum capacity for that fluid. It is useful for operations that require verifying if a tank can no longer accept additional fluid of the same type.

---
---
> ##### ***`isTankFull(SingleVariantStorage<FluidVariant> tank)`***

Determines whether a specified fluid storage tank is full. This method checks if the tank has reached its maximum capacity for the fluid it currently contains. It is useful for operations that require verifying if a tank can no longer accept additional fluid of the same type.

---
---
> ##### ***`isTankFull(Storage<FluidVariant> tank)`***

Determines whether a specified fluid storage tank is full. This method checks if the tank has reached its maximum capacity for the fluid it currently contains. It is useful for operations that require verifying if a tank can no longer accept additional fluid of the same type.

---
---
> ##### ***`hasCapacityForBucket(Inventory inventory, int slot, SingleVariantStorage<FluidVariant> tank)`***

Determines whether a specified fluid storage tank has enough capacity to accept a full bucket worth of fluid. This method checks if the tank can accommodate the additional fluid amount equivalent to a bucket, ensuring that the transfer can occur without exceeding the tank's capacity. It is useful for operations that involve transferring a bucket of fluid into the tank and verifying that there is sufficient space available.

---
---
> ##### ***`hasCapacityForBucket(Inventory inventory, int slot, Storage<FluidVariant> tank)`***

Checks whether a specified fluid storage tank has enough capacity to accept an additional amount of fluid equivalent to a full bucket. This method ensures that the tank can accommodate the fluid without exceeding its capacity, which is crucial for operations involving fluid transfer from an inventory to the tank.

---
---
> ##### ***`getFluidStorage(World world, BlockPos pos, Set<BlockPos> exceptions, Direction direction)`***

Retrieves the fluid storage at a specified position in the world, considering a given direction and a set of exceptions. This method attempts to access the fluid storage located at the specified `BlockPos` within the provided `World`. It takes into account the direction from which the storage is accessed, allowing for directional fluid interactions. Additionally, it considers a set of exception positions, which are locations that should be ignored during the search for fluid storage. This can be useful in scenarios where certain blocks should not be interacted with, such as when avoiding recursive or redundant checks.

---
---
> ##### ***`getAllFluidStorages(World world, BlockPos pos, Set<BlockPos> exceptions)`***

Retrieves all fluid storages surrounding a specified position in the world, excluding specified exceptions. This method scans the area around the given `BlockPos` within the provided `World` to find all available fluid storages. It returns a list of fluid storages that are accessible from the specified position, while excluding any positions listed in the `exceptions` set. This is useful for operations that require interaction with multiple fluid storages in the vicinity, such as fluid distribution or collection systems.

---
---
> ##### ***`convertDropletsToMb(long droplets)`***

Converts a specified amount of fluid measured in droplets to milli-buckets. In the context of Fabric MC, droplets are the base unit of fluid measurement, and this method provides a way to translate that measurement into milli-buckets, which are commonly used in fluid GUI information. This conversion is essential for ensuring consistency across mods for showing different fluid amounts in systems and APIs that may use varying units of measurement.

---
---
> ##### ***`convertMbToDroplets(long mb)`***

Converts a specified amount of fluid measured in milli-buckets to droplets. In the context of Fabric MC, milli-buckets are not a used unit of fluid measurement, and this method provides a way to translate that measurement into droplets, which are the base unit. This conversion is essential for ensuring compatibility and consistency across different fluid systems and APIs that may use varying units of measurement.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Storage<FluidVariant> storage)`***

Spreads fluid from a `FluidStorage` to adjacent `FluidStorage` instances in the world, It checks each direction for available fluid storage and attempts to distribute fluid evenly. If the fluid amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Storage<FluidVariant> storage, boolean equalAmount)`***

Spreads fluid from a `FluidStorage` to adjacent `FluidStorage` instances in the world, It checks each direction for available fluid storage and attempts to distribute fluid. The distribution can be equal or max possible per side, depending on the `equalAmount` flag. If the fluid amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Set<BlockPos> exceptions, Storage<FluidVariant> storage)`***

Spreads fluid from a `FluidStorage` to adjacent `FluidStorage` instances in the world, excluding specific positions defined in the exceptions set. It checks each direction for available fluid storage and attempts to distribute fluid evenly. If the fluid amount changes, it updates the block entity state accordingly.

---
---
> ##### ***`spread(BlockEntity blockEntity, World world, BlockPos pos, Set<BlockPos> exceptions, Storage<FluidVariant> storage, boolean equalAmount)`***

Spreads fluid from a `FluidStorage` to adjacent `FluidStorage` instances in the world, excluding specific positions defined in the exceptions set. It checks each direction for available fluid storage and attempts to distribute fluid. The distribution can be equal or max possible per side, depending on the `equalAmount` flag. If the fluid amount changes, it updates the block entity state accordingly.

## AbstractFluidBlock Class

This class serves as a base class for blocks that interact with fluids within the game world. This class extends the `Block` class and implements the `BlockEntityProvider` interface, allowing it to provide block entities that can perform  additional logic or store data.

The class already has a CODEC attribute and handles returning that codec. Also, the this class already handles getTicker to tick an instance of ITickBE and handles syncronization of data over the network. The `createScreenHandlerFactory` is already implemented and normally you shouldn't need to override this method. Finally, it handles comparator by default with the implementation of `hasComparatorOutput` and `getComparatorOutput` methods. Under normal cases, you shouldn't need to override any of these methods.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, this class is already handling the needed behavior to interact with a fluid container block when you have an empty bucket or a bucket of fluid in hand. The functionality is happening in the <strong>onUseWithItem</strong> method. In case you need to override this method, don't forget to call supe (preferably at the start of the method) to handle this interaction without a problem. 
</div>

## AbstractFluidBE Class

The `abstract` extension of `UpdatableEndTickBE` that implements `ITickSyncBE` and `IWrappedFluidProvider`. This block entity will handle fluid storage and syncronization of fluid storage data. The fluid storage is a wrapped storage that can handle directional storage.

---
---
> ##### ***`addFluidStorage(long capacity)`***

Adds a synced fluid storage with the given capacity and assign the block entity to this new storage.

---
---
> ##### ***`getFluidStorage()`***

Retrieves the wrapped fluid storage instance.

---
---
> ##### ***`readNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Reads the block entity's data from an NBT compound. This method is responsible for deserializing the block entity's state, including its fluid storage, from the provided NBT data.

---
---
> ##### ***`writeNbt(NbtCompound nbt, RegistryWrapper.WrapperLookup registries)`***

Writes the block entity's data to an NBT compound. This method is responsible for serializing the block entity's state, including its fluid storage, into the provided NBT compound.

---
---
> ##### ***`getScreenOpeningData(ServerPlayerEntity player)`***

Retrieves the data necessary for opening a screen on the client side. This method is used to provide the client with the position of the block entity when a screen associated with this block entity is opened. The position is encapsulated in a `BlockPosPayload` object, which is sent to the client to ensure that the correct block entity is referenced.

---
---
> ##### ***`getDisplayName()`***

Retrieves the display name of this block entity. This name is used to represent the block entity in the user interface, such as in the title of a screen or in tooltips. By default, this method returns null, indicating that no specific display name is set for this block entity. Subclasses can override this method to provide a custom display name that is more descriptive or context-specific, enhancing the user experience by providing meaningful information about the block entity.

---
---
> ##### ***`createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player)`***

Creates a screen handler for the block entity, which is used to manage the interaction between the player's inventory and the block entity's inventory. This method is called when a player opens the block entity's screen, allowing for the synchronization of inventory data between the client and server. By default, this method returns null, indicating that no specific menu is created for this block entity. Subclasses can override this method to provide a custom screen handler that facilitates interaction with the block entity's inventory or other functionalities.

Example usage:
```java
@Override
public ScreenHandler createMenu(int syncId, PlayerInventory playerInventory, PlayerEntity player) 
{
    return new SomeScreenHandler(syncId, playerInventory, this);
}
```
