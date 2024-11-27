## Ji Config

The [Config](https://github.com/drkhodakarami/JiConfig) library. This library contains 4 class, one enum and one interfaces. Your main entry to the whole system will be the abstract `Config` class.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiConfig) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/config/) repository for the project.

## ConfigKeyCasing

This enum is used internally to set the letter casing for the key section in your config file. If you use the `load(ConfigKeyCasing casing)` method on `Config` class, you can set your own letter casing instead of the default one.

```java
\\original key "Some.Key.String", casing "ALL_UPPER_CASE", result:
SOME.KEY.STRING=....
\\original key "Some.Key.String", casing "ALL_LOWER_CASE", result:
some.key.string=....
\\original key "SomeKeyString", casing "NO_CHANGE", result:
SomeKeyString=....
```

## IConfigProvider

This interface is used internally by the system.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to implement or extend this interface. This interface is for config system's internal use only.
</div>

## ConfigRequest class

This class is the request for a config object. It holds different important values like the file, the file name, the provider that is instance of `IConfigProvider`, the final `BaseConfig` and the final config string.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod or call any methods from this class and store the return values. This class is for config system's internal use only.
</div>

---
---
> ##### ***`ConfigRequest(File file, String filename)`***

The constructor of the class.

---
---
> ##### ***`getFile()`***

This method returns the `File` associated to the config system.

---
---
> ##### ***`getFilename()`***

This method returns the `Filename` associated to the config system.

---
---
> ##### ***`provider(IConfigProvider provider)`***

This will return an instance of `ConfigRequest` that is being instantiated in the system before hand.

---
---
> ##### ***`request(String modId, ConfigKeyCasing casing)`***

This method will return an instance of `BaseConfig` that is being instantiated before hand with the proper settings for the Mod ID and the key letter casing. Normally you don't need to call this method and save the return value because this method is being used internally by the system.

---
---
> ##### ***`getConfig()`***

This method will return a string containing all the config comments, key/pair values being set in it before hand.

## ConfigProvider class

This is the config provider for the system. This class is responsible of keeping the string of the whole config file and a list of key/value pairs for the config.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod or call any methods from this class and store the return values. This class is for config system's internal use only.
</div>

---
---
> ##### ***``***

## BaseConfig class

text

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod or call any methods from this class and store the return values. This class is for config system's internal use only.
</div>

---
---
> ##### ***`getConfigList()`***

This method returns the `List<Pair<String, ?>>` that is the main key/value pair stored in the class for fast value retrieving.

## Config class

This is the main helper method in the library. Most of the registry calls for Minecraft need the Mod ID of your mod. This class is a helper one and shouldn't be instantiated. Because of this, it has an internal modID attribute and you need to call the init(String modID) from the start of your main mod's initialization class (alternatively, if you are concerned about race condition between different mods calling the class, you can initialize the Registers at the start of each registry call in different classes, more about this on another page):

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

---
---
> ##### ***``***

Just change the RegistryKeys.BLOCK to anything you need from the RegistryKeys class.

Example usage:
```java

```

---
---

## Your Mod's Config Class