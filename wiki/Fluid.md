## Ji Fluid

The [JiFluid](https://github.com/drkhodakarami/JiFluid) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle fluid containers. It provides abstract classes, wrapped fluid containers, and helper methods that will handle more complex fluid transfer for you. It contains 6 classes, 1 interfaces, and 1 enum. The [JiMachina](https://github.com/drkhodakarami/JiMachina) library is depending on this one.

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

## SyncedFluidStorage Class

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

## WrappedFluidStorage Class

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