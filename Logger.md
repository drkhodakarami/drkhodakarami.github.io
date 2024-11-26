## Ji Logger

The [Logger](https://github.com/drkhodakarami/JiLogger) library. This library contains two classes: LoggerConstants and Logger.

## Depending Your Mod

For installation guide on how to add the dependency, look into the [Readme](https://github.com/drkhodakarami/JiLogger) file of the repository dedicated for this library. You will find all the information you need on how to depend your mod project to this library there. To find the version of the library, you can check the table at the main page of the [wiki](https://drkhodakarami.github.io/) or the [Maven](https://repo.repsy.io/mvn/jiraiyah/jilibs/jiraiyah/logger/) repository for the project.

## LoggerConstants Class

The logger constants is a helper class with two sub classes one for foreground and one for background coloring style. The main class has a single property called RESET.

This class only contains ANSI escape characters that you can add to your strings to style the color of the string in the log consol. The foreground sub class contains the escape characters to change the font color of the log string itself and the background class contains the escpae characters to change the background color of the text in the log consol.

Each time you add an escape character that will change the coloring of the string, at the end of the string you need to add the RESET escape character to forcefully reset the coloring back to the normal color used by the IDE. If you forget to add this escape character, any log output after your custom one, will use the same coloring you set for this specific log!

## Logger Class

The class is not a static helper one. You need to create an instance of this class in your project and use that static instance in your project.

In your main mod initialization class:
```java
public class Main implements ModInitializer
        
        public static String ModID = "your_mod_id";
        public static Logger LOGGER = new Logger(ModID);```

From this point forward, in any place that you need to log something, you need to ```import static jiraiyah.uio.Main.LOGGER;``` and then calling on REFERENCE instance, you can utilize the methods described bellow.


### 