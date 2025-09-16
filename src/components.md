## Apply Button

Calls `apply_setting()` on all components that are in a specific node group.

## Arrow List

A list that allows cycling through options using two arrows on the left and right. Compatible with `int` and `bool` settings.

## Binary Selection

Consists of multiple components that all do the same thing. **Checkbox**, **Switch**, and **Toggle Button** are all binary selection components that simply toggle between a true/false state. Compatible with `bool` settings.

## Input Button

Displays the current value of the assigned input setting and rebinds it when interacted with. Only compatible with `SettingInput` (it technically handles `Array` since input events are serialized into arrays when saving them).

Several global properties can be adjusted in plugin preferences.

## Option List

A drop-down menu that allows the selection of one option. Compatible with `int`.

## Radio List

A group of buttons. Only one button can be selected at a time. You can add `Buttons` or `CheckButtons` to the active list. All buttons must have `toggle_mode` enabled. The index of the button node in the active list is the index of the option (e.g. the first button is `0`, the second one is `1`, and so on). Compatible with `int`.

## Reset Button

Calls `reset_setting()` on all components that are in a specific node group.

## Slider

A slider for choosing a number in a certain range. Edit the child `HSlider` to edit range, step, etc. Compatible with `int` and `float`.

## SpinBox

A text field that only accepts numbers. Edit the child `SpinBox` to edit range, step, etc. Compatible with `int` and `float`.

## Text Field

A simple text field. Compatible with `String`.
