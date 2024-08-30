This guide will show you how to get started with GGS and create a setting menu with all the predefined settings and components.

# Installation

Use one of the following methods to install GGS:

- Simply download the latest release from [Releases](https://github.com/PunchablePlushie/godot-game-settings/releases).
- Install the plugin via the [Godot Asset Library](https://godotengine.org/asset-library/asset) which is also available inside the editor.
- Clone the repository with Git, or download the `main` branch directly with a third-party tool.
- Add the repository as a [Git Submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

> [!NOTE]
> The plugin may not work when first installed. Simply ignore the errors, enable the plugin. Make sure the singleton `GGS` is added to your "Globals/Autoload" (the plugin does this automatically when enabled).<br>
> If the singleton was not automatically added for any reason, you can manually add the following scene and name it "**GGS**". Make sure you add the scene, not the script.<br>
> Singleton Path: `res://addons/ggs/plugin/singleton/ggs.tscn`

# How it Works

GGS uses custom resources along with premade scenes referred to as components to handle things. Each setting of your game is a special resource saved in a specific user-defined directory (`res://game_settings` by default). This directory (and other GGS preferences) can be found in the singleton scene (`ggs.tscn`).

GGS uses the built-in `ConfigFile` class to save setting values in an INI-style file using their defined `section` and `key` properties. Additionally, it automatically cleans up the config file, removing any unused keys/sections and adding the missing ones. This file is saved in the user directory (`user://`) and is named `settings.cfg` by default. The name can be changed through preferences, however, the directory cannot and it'll always be the user directory.

> [!TIP]
> You can open the user directory from the **Project** menu >> **Open User Data Folder**.

# Creating Settings

`ggsSetting` resources hold the logic and the default value for the setting. There are two ways to create a setting:

> [!NOTE]
> Remember that settings must be put in your settings directory to work. Also, you can nest settings in as many directories as
> you want. GGS will recursively load them.

## Creating from a Template

A template is a script that extends `ggsSetting` and has a unique `class_name`. GGS comes with a few premade templates for common game settings which can be found in `res://addons/ggs/templates`. In general, the class names start with `setting` (e.g. `settingAudioMute`, `settingInput`, etc.) This allows you to quickly find them by simply searching for "_setting_" when adding a resource.

So, to create a setting from a template, simply open the create resource dialog, look for "_setting_", choose the template that you want, and give the resource an appropriate name.

Now you can inspect the created resource to set any required properties. Regardless, almost all settings share three properties that need to be set:

| PROPERTY  | DESCRIPTION                                                                                                                                 |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| _section_ | The section name the key will be saved under.                                                                                               |
| _key_     | The key name the value will be saved in.                                                                                                    |
| _default_ | The default value of the setting. This is used when reseting the setting or when creating its key in case the config file does not have it. |

## Creating an Empty Setting

You can create an empty setting by simply creating a `ggsSetting` resource (not choosing any template). After that, simply create a new **tool** script that extends `ggsSetting` and assign it to the created resources. For detailed information on creating your own custom settings, view [Custom Settings](5_custom_settings.md).

# Creating Your Settings Menu

Once you've added your settings, it's time to create your settings menu. GGS does not come with its own premade settings menu, rather it comes with several components that allow the user to interact with the setting and change its value. This gives developers flexibility so they can design a settings menu that fits their specific project.

Components are premade scenes that can be instantiated into your own settings menu as many times as needed. Each component must have an assigned setting that it will keep track of. All components start with `component` to make searching for them easier.<br>
To add a component to your scene, simply instantiate them like any regular scene (default hotkey: `Ctrl+Shift+A`).

Once added, you can right click on the component and choose "**Editable Children**". This will allow you to edit the properties of the component like any regular node.

Each component can only handle a specific type of data. An **Arrow List** for example, can only handle integers or booleans while a **Slider** can only handle floats or integers. So when assigning a setting to components, make sure the type of the setting value is compatible with the component.

Additionally, you can create your own custom components if needed. Visit [Custom Components](6_custom_components.md) for more information. You can also find the premade components at `res://addons/ggs/components`.

> [!WARNING]
> GGS does not keep track of the assigned setting of the components. Meaning, that if you delete a setting that's assigned to a component, that component will still have the deleted setting assigned to it. Running the project in this case may result in unexpected behavior.

## Handling Sound Effects

GGS components allow you to play a sound effect when they gain focus or when they're interacted with. To assign audio files for these effects, open the singleton scene (`ggs.tscn`) and assign audio to the desired `AudioStreamPlayer` node. If no audio is assigned to a player, it will simply play no sound.

# Conclusion

That's the core part of what you need to know to use GGS. Other options or features are self-explanatory or have tooltips that you can read. Most classes also contain doc comments so you can look them up in Godot's built-in documentation feature (default hotkey: `F1`).

You can also check [Settings](3_settings.md) and [Components](4_components.md) to view info regarding individual premade items.

Feel free to open an issue if you need further assistance with the plugin, encounter a bug, or would like some additional features.
