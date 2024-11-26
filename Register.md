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