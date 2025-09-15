## Audio

Changes common properties of a selected audio bus.

### Mute

Changes the `muted` state of the selected audio bus.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | bool |
| default | false |
| audio_bus | The audio bus to be affected. |

### Volume

Changes the `volume` of the selected audio bus.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | float, range of [1.0, 100.0] |
| default | 80.0 |
| audio_bus | The audio bus to be affected. |

## Display

Changes common window and display properties.

### Mode

Changes display mode between fullscreen, borderless, and windowed.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | int, exported as an enum |
| default | 2 (Windowed) |
| size_setting | A setting that can handle window size. Used to set the game window to the correct size after state change. If not provided, the window size will not be updated when display mode changes. This is particularly important when the user changes the window size _while_ the game is in fullscreen or borderless modes. |

### Scale

Sets the window scale. The window will be resized by multiplying its dimensions by a flat number.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | int, index of an item from `scales` |
| default | 0 |
| scales | List of available scales.|

!!! warning
    Remember that when making a component, the order of items in the component list must be the same as the order of items in the `scales` property.

### Size

Sets the window size. The window will be resized by setting its size to provided values.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | int, index of an item from `sizes` |
| default | 0 |
| scales | List of available sizes.|

!!! warning
    Remember that when making a component, the order of items in the component list must be the same as the order of items in the `sizes` property.

### Max FPS

Sets the maximum allowed FPS (frames-per-second) of the game.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | int |
| default | 60 |

### VSync

Changes VSync mode.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | int, exported as enum |
| default | 1 (Enabled) |

## Input

Sets the input event of a specific input action (i.e. rebinds an input). The `default` property is read-only and is set automatically whenever you choose an `action` or `event_idx`.

| PROPERTY | DESCRIPTION |
| --- | --- |
| type | Array: [type, id, axis_dir] |
| default | Empty array, changing this value manually is not recommended |
| action | The action to be rebinded. |
| event_index | The index of the input event to be replaced. |

Input events are serialized to arrays using `GGSInputUtils.serialize_event()`. The value of `default` is this array. The array structure is as follows:

- 0, Event Type: The input event type (e.g. `InputEventkey`, `InputEventJoypadMotion`, etc.)
- 1, Event Id: "Id" here refers to the identifying property that indicates what the actual input is. For each supported event type it's:
    - _InputEventKey_: `physical_keycode`
    - _InputEventMouseButton_: `button_index`
    - _InputEventJoypadButton_: `button_index`
    - _InputEventJoypadMotion_: `axis`
- 2, Axis Direction: If the event is an `InputEventJoypadMotion`, the `axis_value` property of the event is stored here.

When the stored value is loaded, the array is converted back to a valid input event using `GGSInputUtils.deserialize_event()`.