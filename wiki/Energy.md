## Ji Energy

The [JiEnergy](https://github.com/drkhodakarami/JiEnergy) is the main abstraction layer for more advanced blocks and block entities in any mod that wants to handle fluid containers. It provides abstract classes, wrapped fluid containers, and helper methods that will handle more complex fluid transfer for you. It contains 6 classes, 1 interfaces, and 1 enum. The [JiMachina](https://github.com/drkhodakarami/JiMachina) library is depending on this one.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, the easiest way to use the library is to extend the block entity from <strong>AbstractFluidBE</strong>, extend your block from <strong>AbstractFluidBlock</strong> and use helper methods provided by FluidHelper class.
</div>

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiEnergy) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/energy/) repository for the project.
