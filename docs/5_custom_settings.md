You can create your own custom settings and templates in GGS.

# Creating a Custom Setting

Simply create a new GDScript. The script must:

- Be a tool script (`@tool` at the top of the script).
- The script must extend `ggsSetting`.
- Must have a method named `apply()` that has a single parameter. This is the value of the setting.
- If you plan to use it as a template for multiple settings, should have a class name (`class_name` keyword).
- If you plan to use it as a template, it's also recommended to set `value_type` and `default` in the script's `_init()`.

Once you're done, you can assign this script to the setting resource of your choice.

# Value Properties

All settings have three value properties: `value_type`, `value_hint`, and `value_hint_string`.

`value_type` is necessary to determine the type of value the setting has. The other can be optionally used to customize the way the `default` property is exported to the Godot inspector.

For example, if your `value_type` is `float`, you can set `value_hint` to be `Range`. And set `value_hint_string` to be `0,100`. This will export `default` as a range between `0` and `100`. For more information, view `_get_property_list()` and `Property.Hint` constants in the Godot docs.

# The Setting Logic

To write the logic for your setting, open the script. The `apply()` method is where your logic should go. Note that it must take a value (the same type defined in `value_type`).

---

Here's a simple example of a VSync setting:

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

You can also check the premade template scripts to see how they're defined.
