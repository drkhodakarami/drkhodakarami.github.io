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
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod but you will call it's methods when you want to add your entries to the config system. To see how you should do this, check the `Your Mod's Config Class` section bellow.
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

This is the main class in the system that holds the `HashMap` for the key/value pairs and provides methods to get the values from the config system. Also, this class is responsible of creating, writing, and reading values of any config class.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, normally you don't need to instantiate this class in your mod, the instantiation is handled internally. When you are creating the config class for your mod, you will utilize methods of this class for reading the values for the config keys of your mod.
</div>

---
---
> ##### ***`of(String modid, String filename)`***

This is a static helper method that is being used internally by the system to create the base instance with bare minimum settings. Check the `Config class` to see how the system is utilizing this method to instantiate the class.

---
---
> ##### ***`get(String key)`***

This method will return a value from the `HashMap`. It does not check for key validation and if the key is not valid, it will return `null` by default.

---
---
> ##### ***`getOrDefault(String key, String def)`***

This method will return a `string` from `HashMap` by the given key. If the key is wrong, it will produce a log error message and assign the provided def value as the default value for the entry. However, in case of using the default value, it will not set the config system's internal value and does not store this value on the file.

---
---
> ##### ***`getOrDefault(String key, int def)`***

This method will return an `integer` from `HashMap` by the given key. If the key is wrong, it will produce a log error message and assign the provided def value as the default value for the entry. However, in case of using the default value, it will not set the config system's internal value and does not store this value on the file.

---
---
> ##### ***`getOrDefault(String key, boolean def)`***

This method will return a `boolean` from `HashMap` by the given key. If the key is wrong, it will produce a log error message and assign the provided def value as the default value for the entry. However, in case of using the default value, it will not set the config system's internal value and does not store this value on the file.

---
---
> ##### ***`getOrDefault(String key, double def)`***

This method will return a `double` from `HashMap` by the given key. If the key is wrong, it will produce a log error message and assign the provided def value as the default value for the entry. However, in case of using the default value, it will not set the config system's internal value and does not store this value on the file.

---
---
> ##### ***`isBroken()`***

Returns the result of the creation and reading of the config file. If there was an error during the creation or loading of the file itself, this will return `true`, otherwise it will return `false`.

---
---
> ##### ***`delete()`***

This method will delete the config file from the system. Normally you should avoid calling this method unless you are sure of what you are doing.

---
---
> ##### ***`createConfig()`***

This is a private method responsible of creating the config file on hard drive.

---
---
> ##### ***`loadConfig()`***

This is a private method responsible of reading the config file from hard drive.

---
---
> ##### ***`parseConfigEntry(String entry, int line)`***

This is a private method responsible of parsing the content of a line in config file and if the parsing was successful, adds the result to the `HashMap`.

## Config class

This is the main entry for the config system that you should utilize in your mod. The class is `abstract` meaning that, you can't directly instantiate this class. Instead, you need to write your own class extending from this one.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, your class should always overriding two methods: createConfigs and assignConfigValues. Normally you don't need to touch any other method from this class and they should work unless you want to expand and change the whole system's functionality.
</div>

```java
public class Configs extends jiraiyah.config.Config
{
    public static int SOME_CONFIG_INT;
  
    @Override
    protected void createConfigs()
    {
        provider.add(new Pair<>("some.config", 128), "The Main Radius!", false, true);//marked as last entry!
    }
  
    @Override
    protected void assignConfigValues()
    {
        SOME_CONFIG_INT = config.getOrDefault("some.config", 128);
    }
}
```

---
---
> ##### ***`Config(String modId)`***

This is the main constructor. Your mod should instantiate the class providing the mod id. Check the `Your Mod's Config Class` for more instruction on this.

---
---
> ##### ***`getConfig()`***

This method returns the internal `BaseConfig` instance just in case you need it anywhere else outside of the config system.

---
---
> ##### ***`load()`***

This method will setup the whole config system. By default it will use ALL_UPPER_CASE for key letter casing.

---
---
> ##### ***`load(ConfigKeyCasing casing)`***

This is the main overload of the previous method. Take a look at the code snippet bellow:

```java
this.config = BaseConfig.of(this.modId, "_config")
  .provider(provider)
  .request(this.modId, casing);
```

As you see, this is how we internally instantiate the `BaseConfig` class. First we call the `of` static method that provides the bare minimum needed. Then we add the provider on top of it that will return a `ConfigRequest` instead of the desired `BasedConfig`. Finally, calling the `request` method on the provided `ConfigRequest`, we get the final instance of the `BasedConfig` with everything setup and ready to be used! This is all done internally and you don't need to worry about it. However, if for any reason, you want to utilize the base structure and create your own config system, it's nice to know this internal mechanic.

---
---
> ##### ***`createConfigs()`***

This method is responsible of creating the entries inside the `ConfigRequest` as provider. You need to override this method in every implementation of config file you have in your mod. Check the code snippet provided at the start of the `Config Class` section.

---
---
> ##### ***`assingConfigValues()`***

This method is responsible of reading the final values from the config system and assing it to your local public static variables. You need to override this method in every implementation of config file you have in your mod. Check the code snippet provided at the start of the `Config Class` section.

## Your Mod's Config Class

First, check the code snippet at the start of `Config Class` section. You need to have your own config class extending the `Config` class. Let's assume that you named it `Configs` as the provided example. Now, in your main mod initialization phase, you need to create an instance of this class and call the methods so that everything gets ready.

```java
public class Main implements ModInitializer
{
    public static String ModID = "your_mod_id";
    public static Configs CONFIGS;
  
    @Override
    public void onInitialize()
    {
        Registers.init(ModID);
      
        CONFIGS = new Configs(ModID);
        CONFIGS.load();
      
        registerItems();
        registerBlocks();
        registerBlockItems();
        //other registring calls
    }
}
```

In the example above, I intentionally let you see that we can initialize the JiRegister library before the config system. The init method of the JiRegister library does nothing other than setting the mod ID for future usage. As you see, the config initializtion is done by calling the `load()` method here. This overload is using ALL_UPPER_CASE by default. If you want to use any other casing, use the other load method from the Config class that accepts the casing enumeration.

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, it's really important when you create and initialize the config system in the mod initialization phase. For example, if you are going to use some config values for registering an Item with some configurable values, your config initialization should happen before item registration. A golden rule is to initialize your config at the start before anything else happens.
</div>