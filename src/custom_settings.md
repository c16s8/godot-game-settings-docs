GGS allows you to create your own custom setting templates and regular settings.

## Creating a Regular Setting

A "regular" setting refers to a setting that will not be used as a template. Something that only needs one instance. To create a new regular setting, first, create an empty resource (simply choose the root **Resource** option in the new resource window).

Now, create a new GDScript somewhere suitable in your project (preferably in your settings directory). This script is where you put the logic of your setting. There are a few things you should take into consideration. The script must:

- Be a tool script (`@tool` at the top of the script).
- Extend the `ggsSetting` class.
- Have a method named `apply()` that takes a single argument. This is the value of the setting and will be passed to `apply()` whenever the setting needs to applied to the game.

This is the bare minimum you need to do. Now you can code the logic of your setting in the `apply()` method.

There are several optional things you can do:

### Custom Init

You can override the `_init()` method to set several useful properties. These properties include:

| PROPERTY             | DESCRIPTION                                                                                                                                                          |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| default              | The initial `default` value of the setting.                                                                                                                          |
| section              | The initial `section` value of the setting.                                                                                                                          |
| key                  | The initial `key` value of the setting.                                                                                                                              |
| read_only_properties | Any property name inside this array will be read-only when the setting is inspected. You may need to reassign the script to the resource for changes to take effect. |

### Setting Logic

As mentioned before, the logic, what actually happens when the setting changes, goes in `apply()`. The `apply()` method must take exactly one argument and that is the new value of the setting. Note that the type of value `apply()` accepts should be the same type defined by `value_type`.

Remember that you can define methods or set additional properties like any script. As long as your script meets the three aforementioned conditions, it's a valid setting script.

Once you've finish your script, you can assign it to the empty resource that you created it previously.

## Creating a Template

Creating a template is exactly the same as creating a regular setting, except this time you give it a `class_name`. This allows it to be instantiated directly from the resource window, similar to the prebuilt templates that already come with GGS.

As a convention, consider starting the class name with **"setting"** so it can be searched for easier.

## Customizing `default` Export

You can use the `value_*` properties to customize how the `default` property is exported to the inspector. This uses Godot's `_get_property_list()`. Note that while you can easily set these inside the editor as well, you may want to set them in the `_init()`, especially if your script is a template.

- `value_type`: The **Variant.Type** of the value (i.e. `bool`, `String`, `Array`, etc.).
- `value_hint`: The `PropertyHint` for the value. This can be used to customize how the property is exported. For example, if the `value_type` is `float` or `int`, you can set the hint to **Range** (which is `PROPERTY_HINT_RANGE`). This will export the `default` property as a range. All of this is done by Godot itself. View `_get_property_list()` for more information on custom exports.
- `value_hint_string`: Used in tandem with `value_hint`. Provides information for some property hints. For example, if `value_hint` is **Range**, hint string can be used to set the range: `0,100`.

````admonish example
Here's how a simple VSync setting would look like:

---

```gdscript
@tool
extends ggsSetting

func apply(value: bool) -> void:
	var vsync_mode: DisplayServer.VSyncMode
	match value:
		true:
			vsync_mode = DisplayServer.VSYNC_ENABLED
		false:
			vsync_mode = DisplayServer.VSYNC_DISABLED

	DisplayServer.window_set_vsync_mode(vsync_mode)
```

---

If we wanted to turn this into a template, the class name would be something similar to `settingDisplayVSync`.
````
