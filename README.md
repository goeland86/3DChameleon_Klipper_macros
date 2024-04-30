# 3DChameleon_Klipper_macros

This repository is relatively simple, and is meant to provide Klipper macros for using Bill Steele's 3D Chameleon.
The original discussion thread for it is here: https://www.3dchameleon.com/forum/support/klipper-macros-for-directly-connected-steppers-from-the-chameleon

## Usage: 

### Requirements
You will need to be able to use 2 additional stepper motor outputs on your controller. If you don't have those, consider getting an additional MCU board, like one of the ERCF boards, or a cheaper controller with 2 stepper outputs (RAMPS would be overkill with 5 steppers, but a BTT SKR Mini E3 would do). You'll only need the two stepper outputs for this to work.

1. Take the `chameleon.cfg` file and place it in your `printer_data/config` folder. At the top of the file are two stepper definitions with TMC2209 sections, annotated with `# S6` and `# S7`

__You HAVE to edit those.__ The definitions provided here are voluntarily incomplete, as your controller will likely differ from my own. I use a [Recore](https://www.iagent.no/product/recore/) with two extra stepper drivers called [ReStep](https://www.iagent.no/product/restep-a1/) - and my config matches that controller and those stepper drivers. You don't need to have that hardware to use these macros. You just need to set up your two drivers to match.
I left the rotation distances as those __will__ need to match, specifically for the selector gear's position.

2. Open the `printer.cfg.addon` file - notice there's a line saying `[include chameleon.cfg]`. Copy that line, and include it in your `printer.cfg` file, anywhere that's before the `#*# <----SAVE_CONFIG ---->` line.

3. Restart Klipper, see if it loads the macros correctly.

4. Test the macros:

*  Run the 3D chameleon empty. Run TOOL_INIT first. See if it rotates in the proper direction (should be counter-clockwise). It should hit the hard endstop and lock for a bit. That's how Bill zeroes it, and it's why the current was set to 0.3A, so you don't apply too much torque. TOOL_INIT also selects T0 as the first filament.

* Try CHAMELEON_LOAD, see if the extruder spins in the right direction to load filament towards the printer's extruder.

* Try the same T2 - the other side of the extruder's drive gear.

* Roughly measure the length of tubing from the drive gears of your printer's extruder to the retracted position of one of the two further PTFE junctions on the Y-splitter. Take that value in mm and enter it in `chameleon.cfg` in the TOOL_INIT macro for the `variable_ysplit_distance` value. This will determine the retraction and insertion distances for the filament load and unload macros.