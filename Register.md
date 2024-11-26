## Ji Register

The [Register](https://github.com/drkhodakarami/JiRegister) library. This library contains one class and three interfaces.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiRegister) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/register/) repository for the project.

## IArmorFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering armor items into the Minecraft's registry system.

## IBlockItemFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering block items into the Minecraft's registry system.

## IToolFactory

This interface is used internally by the Registers class. Normally you shouldn't implement it into any other class. The perpouse of this interface is to provide a factory for registering tool items into the Minecraft's registry system.

## Registers class

This is the main helper method in the library. Most of the registry calls for Minecraft need the Mod ID of your mod. This class is a helper one and shouldn't be instantiated. Because of this, it has an internal modID attribute and you need to call the init(String modID) from the start of your main mod's initialization class:

```java
public class Main implements ModInitializer
{
        public static String ModID = "your_mod_id";
  
        @Override
        public void onInitialize()
        {
                Registers.init(ModID);
        }
}
```

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, every registring of Items, Blocks, Block Entities, or any other entry into Minecraft's registry system, should happen after this call for the init method on Registers. If you mess things up for the order of operation, the default value for the Mod ID will be `default` and not only you will get warnings or errors about missing stuff in the consol, also the models and textures will not work properly any more.
</div>

The registers class has many sub classes, each handling dedicated section of registry entry.

##### getKey(String name, RegistryKey<? extends Registry<T>> registryKey)

This method will return a RegistryKey for the given type. This value is needed for registering anything in the Minecraft registry system (from 1.21.3).

You can get any registry key like this:

```java
RegistryKey<Block> key = getKey(name, RegistryKeys.BLOCK);
```

Just change the RegistryKeys.BLOCK to anything you need from the RegistryKeys class.

### Block Sub Class

This sub class handles registering every `block` into the Minecraft's registry system.

### Entities Sub Class

This sub class handles registering every `entity` (including block entity) into the Minecraft's registry system.

### Item Sub Class

This sub class handles registering every `item` into the Minecraft's registry system.

### BlockItem Sub Class

This sub class handles registering every `block item` into the Minecraft's registry system.

### Recipe Sub Class

This sub class handles registering every custom `recipe` into the Minecraft's registry system.

### ComponentType Sub Class

This sub class handles registering every custom `data component` type into the Minecraft's registry system.

### StatusEffect Sub Class

This sub class handles registering every custom `status effect` into the Minecraft's registry system.

### Datagen Sub Class

This sub class does not register anything into the Minecraft's registry system however, it's responsible of adding helper methods related to `datagen`.