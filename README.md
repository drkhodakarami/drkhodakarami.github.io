# Ji Libs

This is the wiki pages for all of my library system developed for Fabric MC Modding. I call the collection JiLibs but they are modular libraries that can be used separately (with some of them depending on others) or side by side each other in a project.

These libraries include:

- [Ji Logger](https://github.com/drkhodakarami/JiLogger): A wrapper library around logging system provided by minecraft. This wrapper and it's helper methods lets you log information with more style flexibility and with ease.
- [Ji Register](https://github.com/drkhodakarami/JiRegister): This library is all about registring Items, Blocks, and other entries into Minecraft Registry System.
- [Ji Config](https://github.com/drkhodakarami/JiConfig): A wrapper library around [Simple Config](https://github.com/magistermaks/fabric-simplelibs) with more flexibility and more methods that will let you fully customize the look of your config ini files for your mod.
- [Ji Reference](https://github.com/drkhodakarami/JiReference): This library is the main reference for most of my mods. It will produce some base methods like translate, and identifier that I use in almost all of the mods I develop.
- [Jira Lib](https://github.com/drkhodakarami/JiraLib): This library adds the base abstraction layer for most of the mods I develop. The libraries bellow are dependant to this one.
- [JInventory](https://github.com/drkhodakarami/JInventory): As the name suggests, this library is a wrapper around the Item Inventory API provided by fabric for Minecraft Modding.
- [Ji Fluid](https://github.com/drkhodakarami/JiFluid): The wrapper around the fluid API provided by fabric for Minecraft Modding.
- [Ji Energy](https://github.com/drkhodakarami/JiEnergy): The wrapper around the Energy API provided by Tech Reborn Team for Fabric MC
- [Ji Machina](https://github.com/drkhodakarami/JiMachina): This library uses the wrappers provided by JInventory, JiFluid, and JiEnergy to add more abstraction on top for most of the advanced block entity use cases. This library is dependant to Jira Lib, JInventory, Ji Fluid, and Ji Energy.

## Latest Versions:

|Library    |Version        |
|-----------|---------------|
|JiLogger   |`1.1.0+MC-1.21.4`|
|JiConfig   |`1.1.0+MC-1.21.4`|
|JiReference|`1.1.0+MC-1.21.4`|
|JiRegister |`1.1.0+MC-1.21.4`|
|JiraLib    |`1.1.0+MC-1.21.4`|
|JInventory |`1.1.0+MC-1.21.4`|
|JiFluid    |`1.0.0+MC-1.21.3`|
|JiEnergy   |`1.0.0+MC-1.21.3`|
|JiMachina  |`1.0.0+MC-1.21.3`|
