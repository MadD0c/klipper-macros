<h1 align="center">
Icarus
</h1>

These are the klipper macros for my pantheon of printers. 
I initially started working on them with my first printer - Theseus (Kliperized Ender3 V2). Since construction of Deadalus (Voron Trident 300mm) I have made numerous extensions to make it more versatile and adaptable. 

Settings and print variables are all managed via macro variables. This allows them to be adjusted as needed at the time of print or on the fly.

The foundation for this collection (as well as the inspiration) comes from [Rootiest's Zippy-Klipper-Config](https://github.com/rootiest/zippy-klipper_config). I have supplimented his work with: 
- Compatability with the complete version of [KAMP](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging)
- A notification system which works directly with moonraker's notify
- LED templating by [digitalninja-ro](https://github.com/digitalninja-ro/klipper-neopixel)
- Nozzle cleaning macros from [edwardyeeks Decontaminator Purge Bucket Nozzle Scrubber Mod](https://github.com/VoronDesign/VoronUsers/tree/master/orphaned_mods/printer_mods/edwardyeeks/Decontaminator_Purge_Bucket_%26_Nozzle_Scrubber)

## Requrements
For now this is fully contained.
The only external requirement is the install of [KAMP](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging). This repository also needs to be cloned into your `printer_data/config`

## Installation
Clone this repository into your `printer_data/config`
```
git clone https://github.com/MadD0c/Icarus.git
```
Add below to your `printer.cfg` file.
```
[include Icarus/Icarus.cfg]
```
## Configuration
### Printer Setup
All settings available to alter and customize are available in in the `settings.cfg` file in the repository root.
### Slicer Setup
Add these to your slicer profiles. This is compatable with the Slic3r family (PrucerSlicer, SuperSlicer and OrcaSlicer).
##### Start G-code
```
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=extruder_temp VALUE={first_layer_temperature[initial_extruder]}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=bed_temp VALUE={first_layer_bed_temperature[initial_extruder]}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=total_layer_count VALUE={total_layer_count}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=nozzle_size VALUE=0.4 ; Manually set 
START_PRINT
```
##### End G-code
```
END_PRINT
```
##### Before layer change
```
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=layer_num VALUE={layer_num}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=layer_z VALUE={layer_z}
LAYER_CHANGE_BEFORE
```
##### After layer change
```
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=layer_num VALUE={layer_num}
LAYER_CHANGE_AFTER
```
##### Change Filament G-code
```
M600
```
##### Filament preset
```
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=material_type VALUE={filament_settings_id[0]}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=material_color VALUE={filament_colour}
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=nevermore VALUE=False
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=use_scrubber VALUE=False
SET_GCODE_VARIABLE MACRO=_printcfg VARIABLE=chamber_temp VALUE={chamber_temperature}
```
It would be possible to set this up in Ultimaker Cura. Unfortunately due to its issues with layer change placeholders and commands as well as the limited UI, doing such would require post processing plugin.
## Updating
The easist to update the collection is via Moonraker Update Manager. Add below to your `moonraker.cfg`
```yaml
[update_manager Icarus]
type: git_repo
origin: https://github.com/MadD0c/Icarus.git
path: ~/printer_data/config/Icarus
managed_services: klipper
```

## To-Do
I have a few upcoming plans for improving this collection. Unfortunatly I have not got a timeline for now as it all depends on me implimenting them on Daedalus
- Add Support for [NozzleChanger](https://github.com/garethky/change-nozzle-klipper-extra)
- Add Support for [Spoolman](https://github.com/Donkie/Spoolman)
- Add Support for [ERCF](https://github.com/EtteGit/EnragedRabbitProject)