This quick guide will walk you through creating a simple settings menu with prebuilt settings and components that come with GGS out-of-the-box.

# Installation

You can install GGS through one of the following methods:

- Download the latest release from [Releases](https://github.com/PunchablePlushie/godot-game-settings/releases).
- Install the plugin via the [Godot Asset Library](https://godotengine.org/asset-library/asset), also available inside the Godot editor.
- Clone the repository with Git, or download the `main` branch directly with a third-party tool.

Regardless of the method, make sure the plugin files are inside a folder called **ggs** inside your **addons** directory, like so:

```
res://
  ├ addons
  | ├ ggs
  | └ //other addons...
  └ //other project files...
```

Once done, you can enable the plugin from **Project → Project Settings → Plugins**.

```admonish warning
The plugin may not work when first installed. GGS relies on a singleton to function. When the files are initially added, Godot may try to parse them while this singleton is yet to be added to the engine. Simply ignore the errors and enable the plugin. Make sure the singleton **GGS** is added to your **Globals → Autoload**.

The singleton is added automatically when the plugin is enabled. If this does not happen, you can manually add the following scene and **name it "GGS"**. Make sure you add the scene, not the script.

Singleton Path: `res://addons/ggs/plugin/singleton/ggs.tscn`
```

# How it Works

GGS uses custom resources to store essential information regarding each game setting:

- The default value of the setting which can be almost any of the built-in engine types.
- The section and key names used to store the setting value on disc. The built-in `ConfigFile` class is used to do this as it automatically converts built-in types when saving/loading data.<br>
  All data is stored in the `user://` directory. For more information about user directory, view [Data Paths](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html#accessing-persistent-user-data-user) on the official Godot documentation.
- The logic of the setting, as in, what should happen when the setting value is changed. This is written in GDScript.

To allow the user to interact with settings and change their value, we need user interface elements. GGS provides several components that are simple prebuilt scenes. You can use these components to build a fully functioning in-game settings menu.

These components are all `Control` nodes, hence they offer flexibility when designing your menu. You can customize them like any other node (e.g. change their size flags, theme, etc.). For more information on using themes, view [GUI Skinning](https://docs.godotengine.org/en/stable/tutorials/ui/gui_skinning.html) on the official Godot documentation.

```admonish tip
You can open your project's user directory from **Project → Open User Data Folder**.
```

```admonish tip
You can access GGS preferences by opening the singleton scene (`ggs.tscn`) and inspecting the root **GGS** node.

Make sure to reload the singleton (disable/enable it through Project Settings) if you've changed `save_file` or `settings_dir`.
```

# Creating a Setting

GGS loads setting resources in a specific directory referred to as the "settings directory". This directory can be changed in preferences. By default, it's `res://game_settings`.

In the filesystem dock, right-click your settings directory and choose **Create New → Resource...**. In the newly opened window, search for the word "setting". From the list of available resources, choose **settingDisplayFullscreen**. Give this new resource an appropriate name and save it.

You just created a new setting from a _template_. Templates can be used to create multiple instances of the same setting with different properties. This is useful when the logic of the setting is constant but its properties need to change.

Inspect the newly created setting resource to view its properties in the inspector. In addition template-specific properties, there are a few properties that are shared among all settings, including:

| PROPERTY | DESCRIPTION                                                    |
| -------- | -------------------------------------------------------------- |
| default  | This is the default value of this setting.                     |
| section  | The section name that will be used to store the setting value. |
| key      | The key name that will be used to store the setting value.     |

A setting can have an empty `section`, but not an empty `key`. If `key` is empty, the setting's value will not be saved. Go ahead and give this setting an appropriate key name. You can also change the section name if you like.

The **Value Properties** group is used to customize the way the `default` property is exported to the inspector. You don't need to worry about these properties when using templates so ignore them for now.

```admonish info
While the prebuilt settings cover the very basic and general configurations of a game, it does not cover things specific to your own unique project. GGS supports creating custom settings. For more information, view [Custom Settings](custom_settings.md).
```

# Adding a Component

Now that we have a setting, let's make it so the player can interact with it and change its value.

Create a new scene and add a `Control` node as its root. As said before, GGS components are simply scenes that can be instantiated. So, open the instantiate window and search for **"component_switch"**. Select the newly added component to inspect it.

Notice the `setting` property. This is the setting resource associated with the component. Assign the setting you created recently to the component by either drag-and-dropping the resource onto the `setting` property or choosing **Quick Load...**.

```admonish bug
Only the setting resources created from a template show up in the **Quick Load** menu. If your custom setting simply extends `ggsSetting` without providing a `class_name`, it will not be available in this menu. Use other methods to assign the setting in this case (i.e. using **Load** or drag-and-dropping).
```

The root of all GGS components is a `MarginContainer`. To access the underlying nodes (the `CheckButton` in this case), you should enable **Editable Children**. Right-click on the component, and enable **Editable Children**. Now you can select underlying the **Btn** node and change any of its properties. For now, just enter an appropriate `text`.

Now try running the scene. Interact with the component during run time and you may notice that something is wrong. Nothing is happening when you interact with it. Stop the game and inspect the component in the scene tree again. Notice the `apply_on_changed` property. When set to `true`, the component will apply the associated setting immediately. Otherwise, you'll have to use another component called an [Apply Button](./components/apply_button.md) to apply settings.

Set `apply_on_changed` to true. Rerun the scene and change the setting. This time, the game window should successfully toggle between fullscreen and windowed states.

# Handling Sound Effects

Components allow you to play a sound effect on specific events such as gaining focus or when they're interacted with. To assign audio files for these effects, open the singleton scene (`ggs.tscn`) and set the properties of the desired `AudioStreamPlayer` nodes. If no audio is assigned to a player, it won't play any sound.

# Conclusion

This was the core of making a setting and assigning it to a component. To create your setting menu, repeat the process for every setting your game has.

Most properties and classes contain doc comments, meaning Godot can generate documentation for them. Try looking them up in the built-in documentation using **F1**. You can also check the references from the navigation panel to the left.

View [Custom Settings](custom_settings.md) to learn how you can make your own settings. GGS also supports creating [Custom Components](custom_components.md).
