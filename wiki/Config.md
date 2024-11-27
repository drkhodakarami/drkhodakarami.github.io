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
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod but you will call it's methods when you want to add your entries to the config system. To see how you should do this, check the [Your Mod's Config Class] section bellow.
</div>

---
---
> ##### ***`getConfigList()`***

This method returns the `List<Pair<String, ?>>` that is the main key/value pair stored in the class for fast value retrieving.

---
---
> ##### ***`get(String namespace)`***

This method returns the cached string of the whole config file as a string.

---
---
> ##### ***`addPair(Pair<String, ?> pair, String comment)`***

This method adds a key/value pair with the addition of inline comment for the pair. By default it will add a new line at the end of the line.

---
---
> ##### ***`addPair(Pair<String, ?> pair)`***

This method adds a key/value pair without any inline comments. By default it will add a new line at the end of the line.

---
---
> ##### ***`addPair(Pair<String, ?> pair, String comment, boolean addNewLine)`***

This method adds a key/value pair with the addition of inline comment however, it checks the flag for addition of a new line at the end of the line of not.

---
---
> ##### ***`addPair(Pair<String, ?> pair, boolean addNewLine)`***

This method adds a key/value pair without any inline comment however, it checks the flag for addition of a new line at the end of the line of not.

---
---
> ##### ***`addPair(Pair<String, ?> pair, String comment, boolean addNewLine, boolean isLast)`***

This method is an overlay for the previous one with the addition of another flag to check if the current entry is the last entry in the whole system and file or not. If the entry is not the last one, the file will have an empty line after the entry if you have set the add new line flag to true. However, if it's not the last entry and you didn't want the empty line and set the add new line flag to false, after finishing the current line, it will simply return to the start of the new line without any empty line between entries. On the other hand, if it's the last entry in the file, there won't be any action to return to a new line when the current line is finished. Every overload method mentioned above, will internally call this method with the isLast as `false`. If you want to add the last entry to the config system, you should manually call this method and set the is last to true.

---
---
> ##### ***`addPair(Pair<String, ?> pair, boolean addNewLine, boolean isLast)`***

The overload for the previous method without the addition of inline comment. Read the previous method for the flags and how they work.

---
---
> ##### ***`addComment(String comment)`***

Simply adds a comment line in the config without any key/value pair as entry in that line.

---
---
> ##### ***`addNewLine()`***

Adds an extra empty line in the config structure for organizing your config file the way you want.

## BaseConfig class

text

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod or call any methods from this class and store the return values. This class is for config system's internal use only.
</div>

---
---
> ##### ***``***

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