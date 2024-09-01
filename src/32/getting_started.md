This quick guide will walk you through creating a simple settings menu with prebuilt settings and components that come with GGS out-of-the-box.

# Installation

You can install GGS through one of the following methods:

- Download the latest release from [Releases](https://github.com/PunchablePlushie/godot-game-settings/releases).
- Install the plugin via the [Godot Asset Library](https://godotengine.org/asset-library/asset), also available inside the Godot editor.
- Clone the repository with Git, or download the `main` branch directly with a third-party tool.
- Add the repository as a [Git Submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

If you're not installing via the Asset Library, you may need to put the plugin files into the `addons` directory of your project. The correct path will look like so:

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

The core of how GGS works are custom resources simply called "settings". These resources hold the default value and the logic for a specific setting of your game. "Logic" here refers to what should happen when the setting is changed. For example, for a simple fullscreen toggle, the logic includes telling Godot to change the window state. Or for a volume control, the logic is changing the volume of an audio bus.

While the resources hold the key parts of what defines a setting in your game, they don't provide any way for the players to interact with them (i.e. change them). This is where user interface components, or simply "components" for short, come in. Components are simple prebuilt scenes that accept a setting and change its value when they're interacted with.

GGS does not come with an entire prebuilt settings menu. Instead, it provides components to allow developers to create a settings menu with their own style and layout. Of course, because the components are all some type of `Control` node, they can be themed and their properties (such as their layout flags, minimum size, etc.) can be easily changed. For more information on using themes, view [GUI Skinning](https://docs.godotengine.org/en/stable/tutorials/ui/gui_skinning.html) on the official Godot documentation.

Finally, the setting values need to be saved on disc so they can be loaded and used for later game sessions. GGS uses the built-in Godot class `ConfigFile` to achieve this. Config file saves values in an INI-style format, which is a human-readable text format consisting of **sections**. Each section can have several **keys** that can hold a value. Here's how a simple INI file looks like:

```ini
[foo]

some_key = [0, 1, 2, 3]
some_other_key = true

[bar]

some_key = 20
```

The other good thing about using `ConfigFile` is that it supports Godot's built-in types (unlike something like JSON, for example). Meaning, built-in value types such as `Vector2` or `Transform2D` can be saved and loaded without having to convert them first.

GGS saves the config file in the `user://` directory. For more information about user directory, view [Data Paths](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html#accessing-persistent-user-data-user) on the official Godot documentation.

```admonish tip
You can open your project's user directory from **Project → Open User Data Folder**.
```

```admonish tip
You can access GGS preferences by opening the singleton scene (`ggs.tscn`) and inspecting the root **GGS** node.
```

# Creating a Setting

Now that you know how GGS works in general, we can start making.

GGS uses setting resources in a specific directory. We refer to this directory as "settings directory" and it's `res://game_settings` by default. You can change this directory in preferences.

In the filesystem dock, right-click the settings directory and choose **Create New → Resource...**. In the newly opened window, simply search for the word "setting". From the list of available resources, choose **settingDisplayFullscreen**. Give this new resource an appropriate name and save it.

What you just did was creating a new setting from a _template_. Templates can be used to create multiple instances of the same setting with different properties. This is useful when the logic of the setting is the same but what they change is different. For example, an audio volume setting simply changes the volume of an audio bus. What changes is the target audio bus. So by making it a template, we can create a volume setting for each audio bus of our game, without having to repeat the setting logic for each one.

Now, inspect the newly created setting resource. This will show its properties in the inspector. You can see that the template itself has a property called `size_setting` (under `settingDisplayFullscreen` category). Let's ignore that for now.

Under the `ggsSetting` category, there are a few properties that are shared among all settings. We will briefly go through them all:

| PROPERTY | DESCRIPTION                                                        |
| -------- | ------------------------------------------------------------------ |
| default  | This is the default value of this setting.                         |
| section  | This is the section the value of this setting will be saved under. |
| key      | This is the key the value of this setting will be saved in.        |

A setting can have an empty `section`, but not an empty `key`. If `key` is empty, the setting's value will not be saved. Go ahead and give this setting an appropriate key name. You can also change the section name if you like.

You may have also noticed the group **Value Properties**. These properties are used to customize the way the `default` property is exported to the inspector. You don't need to worry about them when using templates so let's ignore them for now.

```admonish info
While the prebuilt settings cover the very basic and general configurations a game may have, it does not cover things specific to your own unique project. GGS supports creating custom settings. For more information, view [Custom Settings](custom_settings.md).
```

# Adding a Component

Now that we have a setting, let's make it so the player can interact with it and change its value.

Create a new scene and add a `Control` node as its root. As said before, GGS components are simply scenes that can be instantiated. So, go ahead and open the instantiate window and search for **"component_switch"**. Select the newly added component to inspect it.

Notice the `setting` property. This is the setting resource associated with the component. Go ahead and assign the setting you created to the component. You can easily do this by either drag-and-dropping the resource to the `setting` property or choosing **Quick Load...**.

```admonish bug
Only the setting resources created from a template show up in the **Quick Load** menu. If your custom setting simply extends `ggsSetting` without providing a `class_name`, it will not be available in this menu. Use other methods to assign the setting in this case (i.e. using **Load** or drag-and-dropping).
```

Now let's give the button a text. The root of all GGS components is a `MarginContainer`. To access the underlying nodes (the `CheckButton` in this case), you should enable **Editable Children**. Right-click on the component, and check the **Editable Children** option. Now you can select the **Btn** node and change any of its properties you see fit. So go ahead and choose an appropriate text for it.

Now let's try running the scene (you can run the current scene with **F6**). Interact with the component during run time and you may notice something is wrong. Nothing is happening when you interact with it. Stop the game and inspect the component in the scene tree again. Notice the `apply_on_changed` property. When set to `true`, the component will apply the associated setting immediately. Otherwise, you'll have to use another component called an **Apply Button** to apply the settings.

So go ahead and set `apply_on_changed` to true. Rerun the scene and change the setting.

# Handling Sound Effects

Components allow you to play a sound effect on specific events such as gaining focus or when they're interacted with. To assign audio files for these effects, open the singleton scene (`ggs.tscn`) and set the properties of the desired `AudioStreamPlayer` nodes. If no audio is assigned to a player, it will simply play no sound.

# Conclusion

This was the core of making a setting and assigning it to a component. To create your setting menu, simply repeat the process for every setting your game has.

Most properties and classes contain doc comments, meaning Godot can generate documentation for them. Try looking them up in the built-in documentation using **F1**. You can also check the references from the navigation panel to the left.

View [Custom Settings](custom_settings.md) to learn how you can make your own regular and template settings. GGS also supports creating your own custom components. View [Custom Components](custom_components.md) for more information.
