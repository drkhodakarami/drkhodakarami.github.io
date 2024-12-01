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

text

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

## EnergyHelper Class

text

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

## AbstractEnergyBlock Class

text

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

## AbstractEnergyBE Class

text

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds

---
---
> ##### ***`getEnergyStorage()`***

Adds
