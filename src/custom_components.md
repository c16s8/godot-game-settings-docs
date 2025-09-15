You can create your own custom components and use them just like the premade ones. Components use a property simply called `value` to keep track of the current setting value. For example, in a simple Toggle Button, this `value` can either be `true` or `false` and it corresponds to the toggle state of the button.

When the user interacts with the component (in this example, toggles the button), the `value` is updated. Then, if apply on changed is enabled, the setting is applied.

## Creating the Component Scene

Components can be stored anywhere as they're just scenes that are instantied. All default components that come with GGS are located at `res://ggs/components`. It is recommended to keep your own components in this directory as well.

To begin, create a new scene. You can add any control nodes you need to this scene, however the root node must be a `MarginContainer`.

## Adding the Main Script

The main script should be assigned to the root `MarginContainer` node and it must fulfil the following requirements:

- It must be a `@tool` script. This allows the component to send configuration warnings.
- It must extend `GGSComponent`.
- When the user interacts with the component (such as toggling a button, changing a slider value, etc.), it should update the `value` property and call `apply_setting()` if apply on changed is enabled. Use `can_apply_on_changed()` to do the check.
- When overriding `reset_setting()`, you must first call the parent method using `super()`.
- When overriding `_ready()`, you must define `compatible_types` of the component.
- To prevent the nodes from running their setting-related code in the editor, use `Engine.is_editor_hint()` as a guard clause.

```gdscript
compatible_types = [TYPE_BOOL, TYPE_INT]
if Engine.is_editor_hint():
  return
```

After the above guard clause, you should get the current value of the setting in `_ready()`. To load setting values from disc, use `GGSSaveManager.load_setting_value()`. Don't forget to update the display elements of the component as well so it actually shows the current value of the setting.

## Class Methods

The following methods come from the base `GGSComponent` class and can be used or overridden when necessary. When overiding them, you must also call the parent method using `super()`.

### apply_setting()

Saves the setting value to the save file and executes the setting logic. You don't generally need to override this, simply call it when you want the component to apply the setting. Always check if apply on changed is enabled before doing so. You can use `can_apply_on_changed()` method to do the check.

This method is also called when a relevant [Apply Button](../components/#apply-button) is pressed.

### reset_setting()

Resets the setting value back to its default and executes the setting logic. You should override this and update your component state in it.

This method is called when a relevant [Reset Button](../components/#reset-button) is pressed.

!!! tip
	You can check the script for prebuilt components that come with GGS for some examples.