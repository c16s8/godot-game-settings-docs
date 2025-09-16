You are not limited to the settings that come with the plugin as GGS allows you to create your own custom settings.

## Creating a Setting Script

The setting script is where you can define your setting properties and its logic. The script must fulfil certain requirements, including:

- It must be a tool script (put `@tool` at the top of the script).
- It must extend the `GGSSetting` class.
- It is recommended to have a class name, and be prefixed with "Setting" (e.g. `class_name SettingFoobar`).
- It must override `_init()` and set the `type` and `default` properties. The `default` property is the default value of the setting, and `type` is its type (corresponding to an engine type such as `TYPE_INT` or `TYPE_FLOAT`).
- Have a method named `apply()` that takes a single argument. Whenever the setting needs to be applied, this method is called and the new setting value is passed to it.

## Customizing Default Export

As mentioned, you must define `default` and `type` properties of the setting in `_init()`. You can also override other properties that may be useful depending on the type of setting you're making. These properties include:

| PROPERTY          | DESCRIPTION |
| ----------------- | ----------- |
| hint              | The property hint for `default`. Use this to customize how the property is exported in the editor. |
| hint_string       | The hint string for the property hint. Always used alongside specific `hint` values. |
| section           | The sectio name used to save and load the setting from the disc. |

## Adding Logic

The logic, or in other words, what actually happens when the setting needs to be applied, goes in the `apply()` method. Note that the type of value `apply()` accepts should be the same type defined in `type`.

Remember that you can define methods or set additional properties like any script. As long as your script fulfils the aforementioned requirements, it's a valid setting script.

Once you've finish your script, you can create a new instance of it via the new resource window.

!!! tip
	For more information about how to use `hint` and `hint_string`, check [`_get_property_list()` on the Godot documentation](https://docs.godotengine.org/en/latest/classes/class_object.html#class-object-private-method-get-property-list).

!!! tip
	You can check the prebuilt settings that come with GGS for practical examples of a setting script. They're located at `res://ggs/scripts` by default