GGS comes with a set of prebuilt scenes known as component. You can instantiate them in any scene necessary.

---

# Apply Button

Calls `apply_setting()` on all components that are in a specific node group.

---

# Arrow List

A list that allows cycling through options using two arrows on the left and right. Compatible with `int` and `bool` settings.

---

# Binary Selection

Consists of multiple components that all do the same thing. **Checkbox**, **Switch**, and **Toggle Button** are all binary selection components that simply toggle between a true/false state. Compatible with `bool` settings.

---

# Input Button

Displays the current value of the assigned input setting and rebinds it when interacted with. Only compatible with `settingInput` (it technically handles `Array` since input events are serialized into arrays when saving them).

Several global properties can be adjusted in preferences.

| PROPERTY        | DESCRIPTION                                                                                                                                                                                                                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _listen_time_   | Time an input button listens for input. When this expires, it automatically stops listening for input.                                                                                                                                                                                                                 |
| _accept_delay_  | Delay before accepting the received input. Mainly used to give enough time to process modifiers for keyboard and mouse events.<br>If you don't plan to accept modifiers, you can set this to its minimum value. If you do, choosing a number that's too low may prevent the users from inputting a key with modifiers. |
| _anim_speed_    | The button's text animation speed when it's listening for input. A higher value means slower animation.                                                                                                                                                                                                                |
| _default_glyph_ | The default glyph type that should be used when no gamepad device is connected or the device is not recognized.                                                                                                                                                                                                        |

```admonish tip
You can view GGS preferences by opening the singleton scene (`ggs.tscn`) and inspecting the root (`GGS`).
```

## Working with ggsGlyphDB

This is a custom resource that can be used to assign textures to mouse buttons and gamepad motions and buttons. It is highly recommended to create and save this on disk and use an `AtlasTexture` to define the individual textures.

The button shows glyphs that correspond to the currently connected gamepad device. If no gamepad is connected or the connected gamepad is not recognized, it uses the glyph type defined in `default_glyph` of preferences.

---

# Option List

A drop-down menu that allows the selection of one option. Compatible with `int`.

---

# Radio List

A group of buttons. Only one button can be selected at a time. You can add `Buttons` or `CheckButtons` to the active list. All buttons must have `toggle_mode` enabled. The index of the button node in the active list is the index of the option (e.g. the first button is `0`, the second one is `1`, and so on) unless `option_ids` is not empty. Compatible with `int`.

---

# Reset Button

Calls `reset_setting()` on all components that are in a specific node group.

---

# Slider

A slider for choosing a number in a certain range. Edit the child `HSlider` to edit range, step, etc. Compatible with `int` and `float`.

---

# SpinBox

A text field that only accepts numbers. Edit the child `SpinBox` to edit range, step, etc. Compatible with `int` and `float`.

---

# Text Field

A simple text field. Compatible with `String`.
