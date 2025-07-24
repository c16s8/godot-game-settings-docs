You can create your own custom components and use them just like the premade ones. Components use a property simply named `value` to keep track of the current setting value. For example, for a simple Toggle Button, this `value` can either be `true` or `false` and it corresponds to the button pressed state.

When the user interacts with the component (in our example, toggles the button), the `value` is updated with the new value (in our example, the new pressed state). Then, if `apply_on_changed` is enabled, the setting is applied.

## Creating the Component Files

Components can be anywhere as they're simply scenes. Create a scene with an appropriate name. The scene root must be a `MarginContainer`. You can any controls you see fit to the scene.

## Adding a Script

Now, you can add a script to your root `MarginContainer` node. This script:

- Must be a `@tool` script. This is to allow them to send configuration warnings when the assigned setting may have an issue.
- Must extend `ggsComponent`.
- When overriding the following `reset_setting()`, you must first call the parent method using `super()`.
- When the user interacts with the component (such as toggling a button, changing a slider value, etc.), you should update the `value` property and call `apply_setting()` if `apply_on_changed` is `true`.

```gdscript
value = new_value
if apply_on_changed:
	apply_setting()
```

- When overriding the `_ready()` method, you should define the `compatible_types` of the component. We don't want the nodes to run their setting-related code in the editor so use `Engine.is_editor_hint()` as a guard to prevent that.

```gdscript
compatible_types = [TYPE_BOOL, TYPE_INT]
if Engine.is_editor_hint():
  return
```

You should also get the current value of the setting in your `_ready()` and update the display elements to reflect the value. The method `GGS.get_value()` can be used to load the current value of the assigned setting from the save file. Simply pass the `setting` property (which holds a reference to the assigned `ggsSetting` resource) to it.

```gdscript
value = GGS.get_value(setting)
# update visual stuff here
```

## Class Methods

The following methods come from the base `ggsComponent` class and can be used or overridden when necessary. When overiding them, however, call them on the parent class with `super()`.

### apply_setting()

Saves the setting value to the save file and executes the setting logic. You don't generally need to override this, simply call it when you want the component to apply the setting. Always check for `apply_on_changed` before doing so.

This method is also called when a relevant [Apply Button](4_components/#Apply-Button) is pressed.

### reset_setting()

Resets the setting value back to its default and executes the setting logic. You should override this and update your component state in it.

This method is called when a relevant [Reset Button](4_components/#Reset-Button) is pressed.

---

Here's a simple example of a toggle button component:

```gdscript
@tool
extends ggsComponent

@onready var _Btn: Button = $Btn


func _ready() -> void:
	compatible_types = [TYPE_BOOL]
	if Engine.is_editor_hint():
		return

	_init_value()
	_Btn.toggled.connect(_on_Btn_toggled)


func reset_setting() -> void:
	super()
	_Btn.set_pressed_no_signal(setting_value)


func _init_value() -> void:
	value = GGS.get_value
	_Btn.set_pressed_no_signal(value)


func _on_Btn_toggled(toggled_on: bool) -> void:
	value = toggled_on
	if apply_on_changed:
		apply_setting()

```
