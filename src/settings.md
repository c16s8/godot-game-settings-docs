GGS comes with a set of common settings that are used in almost all games.

---

## Audio

The following allow you to change the common properties of a certain audio bus. It automatically detects available audio buses for easy selection.

### Mute

Changes the `muted` state of the selected audio bus.

| PROPERTY     | DESCRIPTION       |
| ------------ | ----------------- |
| _value_type_ | `bool`            |
| _default_    | `false`           |
| _bus_name_   | Target audio bus. |

### Volume

Changes the `volume` of the selected audio bus.

| PROPERTY     | DESCRIPTION                   |
| ------------ | ----------------------------- |
| _value_type_ | `float`, range of _0_ – _100_ |
| _default_    | `80.0`                        |
| _bus_name_   | Target audio bus.             |

---

## Display

The following allow you to change the window state or size. All sizes are user-defined.

### Fullscreen

Toggles Fullscreen mode.

| PROPERTY       | DESCRIPTION                                                                                                                                                                                                                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _value_type_   | `bool`                                                                                                                                                                                                                                                                                                                 |
| _default_      | `false`                                                                                                                                                                                                                                                                                                                |
| _size_setting_ | A setting that can handle window size. Used to set the game window to the correct size after its fullscreen state changes. If not provided, the window size will not be updated when the fullscreen turns off. This is particularly important when the user changes the window size _while_ the game is in fullscreen. |

### Scale

Sets the window scale. The window will be resized by multiplying its dimensions by a flat number.

| PROPERTY     | DESCRIPTION                                                                                                                            |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| _value_type_ | `int`, index of an item from `scales`                                                                                                  |
| _default_    | `0`                                                                                                                                    |
| _scales_     | List of available scales. The order of items must be the same as the order of items in the component this setting will be assigned to. |

### Size

Sets the window size. The window will be resized by setting its size to provided values.

| PROPERTY     | DESCRIPTION                                                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| _value_type_ | `int`, index of an item from `sizes`                                                                                                  |
| _default_    | `0`                                                                                                                                   |
| _sizes_      | List of available sizes. The order of items must be the same as the order of items in the component this setting will be assigned to. |

---

## Input

Sets the input event of a specific input action (i.e. rebinds an input). The `default` property is read-only and is set automatically whenever you choose an `action` or `event_idx`.

| PROPERTY     | DESCRIPTION                                            |
| ------------ | ------------------------------------------------------ |
| _value_type_ | `Array`, [`event_type`, `event_id`, `auxiliary_value`] |
| _default_    | `[]`, Read-Only                                        |
| _action_     | The target action from the InputMap.                   |
| _event_idx_  | The target event from the selected action.             |

### Type and ID

Type refers to the input event type (e.g. `InputEventKey` or `InputEventJoypadMotion`). This is used to create the correct `InputEvent` when deserializing the saved value.

ID refers to the property that stores what the actual input is. For each event type, it's as follows:

- **InputEventKey**: `physical_keycode`
- **InputEventMouseButton**: `button_index`
- **InputEventJoypadButton**: `button_index`
- **InputEventJoypadMotion**: `axis`, along with `axis_value` as auxiliary value.
