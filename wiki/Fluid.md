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
  :bulb:<strong>Remember</strong>, the current implementation, version 1.1.2+MC_1.21.3, does not provide default behavior for handling the <strong>FACING</strong> of the block to retrieve proper storage based on the rotation and by default it assumes that the block is facing north. If you want to handle the rotational facing, you need to add your own logic (until we handle it properly ourselves in the library).
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

## FluidHelper Class

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

## AbstractFluidBlock Class

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

## AbstractFluidBE Class

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This

---
---
> ##### ***``***

This