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

<div class="alert alert-dismissible alert-danger">
  :bulb:<strong>Remember</strong>, in the code snippet bellow, the `Logger` class should reference the `jiraiyah.logger.Logger` and no other Logger class available.
</div>

In your main mod initialization class:
```java
public class Main implements ModInitializer
        
        public static String ModID = "your_mod_id";
        public static Logger LOGGER = new Logger(ModID);
```

From this point forward, in any place that you need to log something, you need to `import static project.your_mod_id.Main.LOGGER;` and then calling on REFERENCE instance, you can utilize the methods described bellow. Remember, in this import example, you need to change the project.your_mod_id to fit your project package structure (alternatively, most development softwares give you an option to automatically import the package).

### debug flag

The class methods use a debug flag internally. The debug flag will be set to true only if you are running your mod inside a development environment (for example inside intelliJ IDEA). In other words, when the players add your mod to their game, none of the log messages you send utilizing this class's methods will be seen in the log entries with only one exception, the LogMain method that will log the starting of the mod's initialization phase for your mod. In other words, regardless of where your code is running (from the IDE or a real game instance) the entry for the main mod initialization will always sent to the log file and consol.

### logMain()

This method will be used only once in your main mod's initialization phase like this:

```java
@Override
public void onInitialize(){
        LOGGER.logMain();
}
```

The result will be a log entry in your consol with pink color background and yellow font color with this text `>>> Initializing`.

### log(String message)

This method will simply log the message with magenta color as the font's foreground color and `>>>` at the strat of the message.

### log(String message, String foreground)

This method will accept the color for font's foreground side by side the message. It will log the message using the color with the `>>>` at the start of the message.

### log(String message, String foreground, String background)

This method will accept colors both for font's foreground and message background. It will log the message using the given colors with the `>>>` at the start of the message.

### logError(String message)

This method will send the log message into the consol using `black` as the foreground and `bright red` as the background colors with the `>>>` at the start of the message.

### logWarning(String message)

this method will send the log message into the consol using `black` as the foreground and `bright yellow` as the background colors with the `>>>` at the start of the message.

### logN(String message)

this method will simply log the message without any coloring start with the addition of `>>>` at the start of the message.

### logRGB256(String message, int r, int g, int b)

This method will accept red, green, and blue values for any color even outside of the boundaries of the ANSI escape characters for the font's foreground color. It will log the message with the addition of `>>>` at the start of the message.

### logBackRGB256(String message, int rf, int gf, int bf, int rb, int gb, int bb)

This method will accept red, green, and blue values separately for any color even outside of the boundaries of the ANSI escape characters for the font's foreground and the message's background colors. It will log the message with the addition of `>>>` at the start of the message.