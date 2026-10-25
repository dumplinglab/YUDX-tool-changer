# YUDX Tool Changer — Klipper Example Configuration

> **Work in progress — use at your own risk.** These macros and configuration
> files are an experimental Klipper configuration for the YUDX Tool Changer
> project. They are under active development and have not been
> verified on every printer or hardware setup. Incorrect pin assignments,
> movement limits, dock positions, or macro behavior can damage the printer or
> toolheads. Review and test the configuration carefully before enabling tool
> changes, and supervise initial tests.

This directory provides a starting-point Klipper configuration and macro
example for the YUDX Tool Changer. It includes a two-tool setup, shared
toolhead settings, and a template for adding more tools. It is not a
drop-in configuration: adapt and verify it for your printer and YUDX hardware.

## Installation

1. Create a `toolchanger` directory inside your Klipper configuration
   directory.
2. Place the active configuration files in that directory:
   - `macros.cfg`
   - `toolhead.cfg`
   - `tool_0.cfg`
   - `tool_1.cfg`
   - Any additional tool configuration files made from `tool_X.cfg`
3. Add this include to `printer.cfg`:

   ```ini
   [include toolchanger/*.cfg]
   ```

   **Do not leave the unedited `tool_X.cfg` template with a `.cfg` extension
   inside the included directory.** Klipper's wildcard will include it, and
   its placeholder values (such as `XX.X` and `XXX`) are not a usable
   configuration. Keep the template outside `toolchanger`, or rename it to
   something that does not end in `.cfg` (for example, `tool_X.cfg.example`).
4. Make sure `[save_variables]` is active in `printer.cfg`, or activate and
   configure the example section in `toolhead.cfg`. For example:

   ```ini
   [save_variables]
   filename: ~/printer_data/config/variables.cfg
   ```

5. Configure and verify the settings for your printer and YUDX installation
   before restarting Klipper or attempting a tool change:
   - Replace every example or placeholder pin with a valid pin for your
     controller and connected hardware.
   - Set tool count, dock direction and positions, clearance, push distance,
     offsets, and movement speeds to match your physical setup. Confirm the
     full dock travel path is clear and within printer limits.
   - Check the extruder, heater, temperature-sensor, and fan settings against
     the hardware installed on each tool. Tune extrusion and heater values for
     the specific hardware.
   - Keep tool IDs consecutive, starting at T0, and ensure every tool has its
     matching extruder, variables, dock fan, and Tn macro.
   - Resolve all startup configuration-check errors. Test tool changes at low
     speed while supervising the printer before normal use.

6. `macros.cfg` uses a delayed_gcode that verifies the config as good as possible 
    and prints any inconsistencies to console after firmware restart. make sure 
    there are no errors in this format: `Toolchanger: Error: ...`

## Configuration by file

### `toolhead.cfg`

Contains shared settings in `[gcode_macro _TOOLHEAD_CFG]`, plus the shared
part-cooling fan, hotend fan, probe, optional toolhead hall sensor, and
optional dummy-extruder example.

Set or check these values for your machine:

- `variable_tool_count`: number of tools installed on the printer.
- `variable_dock_direction`: direction of dock engagement (`X+`, `X-`, `Y+`,
  or `Y-`).
- Docking geometry and motion: `variable_dock_position_z`,
  `variable_dock_clearance`, `variable_push_distance`,
  `variable_docking_speed`, and `variable_travel_speed`.
- Extruder mechanism and movement: `variable_extruder_open_dist`,
  `variable_extruder_close_dist`, and `variable_extruder_speed`.
- Travel behavior: `variable_dock_fixed_to_gantry`,
  `variable_z_hop_before_travel`, `variable_z_hop_dist`,
  `variable_retract_before_travel`, and `variable_retract_dist`.
- `variable_cmd_before_docking` and `variable_cmd_after_docking`, if other
  macros or actions are needed around a tool change.
- Hardware pins and settings in `[fan]`, `[heater_fan hotend_fan]`, and
  `[probe]`. 
- The `[heater_fan hotend_fan]` `heater:` list must name every tool's extruder.
- Optional feature: `variable_has_hall_sensor` and the `[gcode_button toolhead_hall_sensor]`
  section if a toolhead sensor is configured.
- Optional feature: A dummy extruder can be configured to allow opening and closing the extruder 
  gears when no tool is loaded and still use min_extrude_temp safety mechanism for the installed tool.
  This feature requires an unused GPIO output pin on any mcu.
  See description below.

### `tool_0.cfg`

Defines the variables, main `[extruder]`, dock cooling fan, optional dock
hall sensor, and `T0` tool-selection macro for tool 0. This file also
configures the shared extruder stepper used by the tool setup.

- `variable_dock_position`: that tool's dock coordinates.
- `variable_nozzle_offset_x`, `variable_nozzle_offset_y`, and
  `variable_nozzle_offset_z`: tool 0 is the reference, so these offsets
  normally remain `0.0`.
- Stepper, heater, and temperature-sensor pins, plus extruder calibration and
  heater tuning values, for the tool 0 hardware.
- The `[heater_fan dock_fan_t0]` pin and heater association.
- Optional feature: `variable_has_hall_sensor` and `[gcode_button dock_hall_sensor_t0]` if tool
  0's dock has a hall sensor.

For a controllable TMC2209 on the tool 0 extruder, the file includes a
commented `[tmc2209 extruder]` example directly below `[extruder]`. Uncomment
it and configure the UART pin and motor current for your driver and wiring.
For different TMC drivers, add the corresponding section accordingly.

### `tool_1.cfg`

Defines the variables, `[extruder1]`, dock cooling fan, optional dock hall
sensor, and `T1` tool-selection macro for tool 1. Configure:

- `variable_dock_position` and the nozzle offsets relative to tool 0.
- The extruder heater and temperature-sensor pins, extrusion limits, nozzle
  and filament diameters, and heater-control/PID values for the tool 1
  hardware. Tool 1 uses the shared extruder stepper configured in
  `tool_0.cfg`.
- The `[heater_fan dock_fan_t1]` pin and its `heater: extruder1` association.
- Optional feature: `variable_has_hall_sensor` and `[gcode_button dock_hall_sensor_t1]` if the
  dock has a sensor.

Replace example pin names and tune hardware-dependent values, including
extruder rotation distance and heater PID values, for your printer. The
provided pins and tuning values are examples, not universal defaults.

### `tool_X.cfg` — template for additional tools

Use this file as the starting point for each additional tool after T1. Copy it
to a new file such as `tool_2.cfg`, then replace every `X` with that tool's
numeric ID. For tool 2, for example, use `_VARS_T2`, `[extruder2]`,
`[heater_fan dock_fan_t2]`, (optional) `[gcode_button dock_hall_sensor_t2]` if needed,
and a `T2` macro that runs `CHANGE_TOOL TOOL=2`.

Also replace the template values:

- `XX.X` and `YY.Y` with the dock's X/Y coordinates.
- `XXX` pin placeholders with valid pins for the extruder heater, temperature
  sensor, and dock fan.
- `heater: extruderX` with the matching extruder name.
- The nozzle, filament, extrusion limits, and PID values with values suitable
  for the tool's hardware.
- Optional feature: Hall effect sensor. Change pin and section name.

IMPORTANT: Add every extruder to the `[heater_fan hotend_fan]` `heater:` list in `toolhead.cfg`.

Each additional extruder uses the shared extruder stepper configured in
`tool_0.cfg`; configure its heater and sensor for the additional tool. Keep
tool IDs consecutive and update `variable_tool_count` in `toolhead.cfg` to the
total number of tools. 

## Optional hardware configuration

The following options have example or placeholder configuration in these files.
They are not a substitute for verifying the hardware and testing the resulting
configuration.

### Hall-effect sensor on the toolhead

! This feature is currently not supported by the macros. !
In `toolhead.cfg`, set `variable_has_hall_sensor: True` and uncomment the
`[gcode_button toolhead_hall_sensor]` section. Set its `pin` to the sensor's
input pin and configure any required pin polarity and debounce settings for
your sensor and board.

### Hall-effect sensors in tool docks

! This feature is currently not supported by the macros. !
The configuration allows a hall-effect sensor to be used at every dock to detect that docking
a tool was successful.
To configure this feature, set `variable_has_hall_sensor: True` in that tool's `_VARS_Tn` section. 
Uncomment that tool's `[gcode_button dock_hall_sensor_tN]` section and set the correct
input `pin` and any required polarity or debounce settings. Leave the variable
`False` when that dock has no sensor.

### Dummy extruder for the no-tool state

The optional dummy extruder allows the extruder mechanism to be operated while
no tool is mounted. To configure it:

1. Set `variable_use_dummy_extruder: True` in `[gcode_macro _TOOLHEAD_CFG]`.
2. In `toolhead.cfg`, uncomment the `[extruderN]` example and replace `N` with
   the value of `variable_tool_count` (the number of real tools).
3. Configure its heater output using a suitable unused, unconnected output pin
   as directed by the example. Check the pin's electrical and board-specific
   requirements before use.
4. Keep the example's `temperature_combined` sensor configuration and
   zero-valued PID settings as appropriate for the dummy extruder; do not
   connect a heater or tune it as a real hotend.

Verify that the dummy section name and settings are accepted by your Klipper
version and hardware before enabling the option.

## Using the tool macros

- `T0`, `T1`, and any added `Tn` macro select the corresponding tool.
- `CHANGE_TOOL TOOL=n` selects a tool by ID.
- `CHANGE_TOOL TOOL=-1` docks the current tool without picking up another.
- `SET_CURRENT_TOOL TOOL=n` records which tool is already mounted; use
  `SET_CURRENT_TOOL TOOL=-1` when no tool is mounted.

Only use `SET_CURRENT_TOOL` to record the actual physical tool state; it does
not perform a dock or pickup.

## Additional features

Hall-effect sensor configuration and the optional dummy extruder are described
above. The following features are planned:

- Complete support for hall-effect sensor detection in the toolhead.
- Complete support for hall-effect sensor detection in individual tool docks.
- Helper macros for tool calibration.

Planned features are not implemented by the current configuration. In
particular, the existing sensor section checks do not provide a complete
sensor-driven safety or tool-detection workflow.
