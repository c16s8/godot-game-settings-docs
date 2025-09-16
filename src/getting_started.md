This quick guide will walk you through creating a simple settings menu with pre-defined settings and components that come with GGS.

## Installation

You can install GGS through one of the following methods:

- Install the plugin via the [Godot Asset Library](https://godotengine.org/asset-library/asset), also available inside the Godot editor.
- Download the latest release from [Releases](https://github.com/PunchablePlushie/godot-game-settings/releases).
- Clone the Github repository.

Once installed, enable the plugin from **Project → Project Settings → Plugins**.

!!! warning

    The plugin may not work when first installed. GGS relies on a singleton to function. When the files are initially added, Godot may try to parse them while this singleton is yet to be added to the engine. Simply ignore the errors and enable the plugin. Ensure the singleton **GGS** is added to your **Project Settings → Globals → Autoload**.

    The singleton is added automatically when the plugin is enabled. If this does not happen, you can manually add the following scene and **name it "GGS"**. Make sure you add the scene, not the script.

    Singleton Path: `res://addons/ggs/core/global_manager/global_manager.tscn`

## How it Works

GGS uses resources to store essential information about each game setting:

- The default value of the setting which can be almost any of the built-in engine types.
- The section and key names used to store the setting value on disc. Godot's own `ConfigFile` class is used to do this as it automatically converts Godot types when saving and loading data.<br>
The setting data is stored at `user://setting.cfg`. For more information about the user directory, view [Data Paths on the official Godot documentation](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html#accessing-persistent-user-data-user).
- The logic of the setting, as in, what should happen when the setting value is changed. This is a GDScript script assigned to the resources.

!!! tip
    You can open your project's user directory from **Project → Open User Data Folder**.

---

To allow the user to interact with settings and change their value, we need user interface elements. GGS provides several components that are simple prebuilt scenes. You can use these components to build a fully functioning in-game settings menu.

These components are all `Control` nodes, hence they offer flexibility when designing your menu. You can customize them like any other node (e.g. change their size flags, theme, etc.). For more information on using themes, view [GUI Skinning on the official Godot documentation](https://docs.godotengine.org/en/stable/tutorials/ui/gui_skinning.html).

!!! tip
    You can access the plugin preferences by inspecting `plugin_settings.tres` located at `res://ggs`.<br>
    Reload the singleton (disable/enable it through the Project Settings) after changing the `settings_dir` property.


## Creating a Setting

When the game starts, the global manager (the GGS singleton) loads and applies the setting resources in a specific directory. This directory is called the "settings directory" and it can be changed in the plugin preferences. By default, it's `res://ggs/game_settings`.

!!! note
    There may already be a `sample_setting.tres` in the settings directory. Feel free to delete that.

As mentioned previously, settings are just simple resources. In the filesystem dock, right-click your settings directory and choose **Create New → Resource...**. In the newly opened window, search for the word "setting". Any resource class that starts with the prefix "setting" is a prebuilt setting that comes with GGS. This is simply a naming convention to make finding setting resources easier.<br>
From the list, choose **settingDisplayMode**. Give it an appropriate name and save it.

Inspect the newly created setting resource to view its properties in the inspector. All settings have three main properties:

| PROPERTY | DESCRIPTION                                                    |
| -------- | -------------------------------------------------------------- |
| default  | This is the default value of this setting.                     |
| section  | The section name that will be used to store the setting value. |
| key      | The key name that will be used to store the setting value.     |

A setting can have an empty `section`, but not an empty `key`. If the `key` is empty, the setting's value will not be saved. Go ahead and give this setting an appropriate key name..

!!! info
    While the prebuilt settings cover the very basic and general configurations of a game, it does not cover things specific to your own project. GGS supports creating your own custom settings. For more information, view [Custom Settings](custom_settings.md).


## Adding a Component

Let's create a corresponding component for the setting we just created.

Create a new scene and add a `Control` node as its root. As said before, GGS components are simply scenes that can be instantiated. So, open the instantiate window and search for **"component_option_list.tscn"**. Similar to settings, all component scenes are prefixed with "component" to make finding them easier.

Select the instantiated scene. Notice the `setting` property. This is the setting resource associated with the component. Assign the setting you created recently to the component by either drag-and-dropping the resource onto the `setting` property or choosing **Quick Load...**.

The root of all GGS components is a `MarginContainer`. To access the underlying nodes (the `OptionButton` in this case), you should enable **Editable Children** through the right-click menu. Now you can select the underlying **Btn** node and change any of its properties. For now, just enter an appropriate `text`.

The display mode setting has three options. So let's add the options to the option button. Note that the order of the items must be the exact same as the order of them in the setting. Meaning, since the options in the setting are Fullscreen, Borderless, Windowed in that order, the option button items must be in that order as well. The display text can be anything however (e.g. it doesn't need to exactly say "Fullscreen" or "Windowed").

Run the scene and change the setting. The game window should successfully change to the selected display mode.

## Handling Sound Effects

Components allow you to play a sound effect on specific events such as gaining focus or when they're interacted with. To assign audio files for these effects, open the global manager scene (`global_manager.tscn`) and set the properties of the desired `AudioStreamPlayer` nodes. If no audio is assigned to a player, it won't play any sound.

## Conclusion

This was the core of making a setting and assigning it to a component. To create your setting menu, repeat the process for every setting your game has.

Most properties and classes contain doc comments, meaning Godot can generate documentation for them. Try looking them up in the built-in documentation using **F1**. You can also check the references from the navigation panel to the left.

View [Custom Settings](custom_settings.md) to learn how you can make your own settings. GGS also supports creating [Custom Components](custom_components.md).
