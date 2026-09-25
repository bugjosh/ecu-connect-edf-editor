# Bugfix Changelog

## Revision 2.1.3 - 2026-09-25

### Affected Areas

- Embedded vehicle and parameter data.
- ReadMethod defaults for new widgets, chart channels, and EDF parsing/saving.
- Parameter picker, vehicle switching, and the compatibility report.

### Fixes and Changes

- ECU Connect only resolves a dashboard parameter when its EcuType, Name, DisplayUnitString, and `ReadMethod` all match the vehicle's LogParam exactly. Revision 2.1.2 applied the VR30TT Gen1 ReadMethod (`CAN_OBD_RR2_Nissan_Gen1_OEM`) everywhere, which broke new widgets on 370Z, GTR, Juke, Frontier, BMW, Ford, and most other vehicles.
- Regenerated the embedded vehicle and parameter data from ECU Connect's own mapping: ECU definition to VehicleId, and each ECU class's LogParams file. The previous data matched vehicles to files by name. Corrections:
  - RZ34 now lists the VR30TT parameters, not the 370Z ones.
  - 350Z Gen2 uses the K-Line file.
  - VR30TT TCM parameters come from the RE7R TCM file the app loads.
  - BMW 8HP automatics and VW DSG vehicles now include their TCM parameters.
  - ECM-only VR30TT and TCM-only GTR IDs list only the ECU they have.
  - Subaru DIT includes the shared DIT file.
  - Mazda DISI Gen1 includes both of its files.
- Every parameter now carries the ReadMethod ECU Connect uses for it, and picking a parameter applies that exact ReadMethod. Each vehicle's default ReadMethod per ECU comes from the same data:
  - GTR: `CAN_OBD_LID_Nissan` (not `CAN_OBD_CID_Nissan_Gen1`).
  - BMW: `DCAN_BMW_CID` (not `DCAN_BMW_PID`).
  - Volkswagen: `CAN_OBD_RR2_Volkswagen_ECM`.
  - Generic OBD: `CAN_OBD_PID`.
  - 350Z Gen1/Gen2: `Multi_LID_Nissan`.
  - VK56VD and 350Z Gen3: `CAN_OBD_LID_Nissan_Legacy`.
  - Nissan TCMs: `CAN_OBD_LID`.
- Every VR30TT ID, including Gen2, RZ34, and the legacy `NissanVR30TT`/`NissanVR30TTGen2`, defaults to `CAN_OBD_RR2_Nissan_Gen1_OEM`. All of their ECU definitions load the same `Nissan_VR30TT_ECM.exml`, and the app never translates Nissan ReadMethods.
- Added RaceROM parameters with their exact ReadMethods from the five RaceROM tables in the app's demo simulator: 370Z, GTR, Ford EcoBoost, BRZ, and GR86.
- Added parameters seen in real sample dashboards with their exact ReadMethods, for example GTR `Boost Target` on `CAN_OBD_CID_Nissan_Gen2` from newer RaceROM builds.
- Removed RaceROM names copied from other vehicles with no source, on vehicles that have a real RaceROM table.
- No VR30TT RaceROM table ships in the app, so the existing VR30TT RaceROM names are kept with an inferred `CAN_OBD_RR2_Nissan_Gen1_RR`, or `CAN_OBD_CID_Nissan_Gen1` for computed channels.
- The parameter picker shows each parameter's ReadMethod and applies the exact row chosen, so parameters that exist in both OEM and RaceROM form (for example BRZ "Engine Load") can be told apart. Committing the Name field keeps the chosen variant.
- New widgets, `+ Add channel` chart channels, and parser/serializer fallbacks resolve the default through the dashboard's `VehicleId` and `EcuType`. On a TCM-only vehicle, new widgets start as `Tcm`.
- Picking a parameter from a different ECU no longer leaves the previous ECU's ReadMethod behind.
- Switching vehicles moves ReadMethods that were the old vehicle's default to the new vehicle's default, including Nissan-to-Nissan switches. Custom ReadMethods that are still valid are kept.
- The compatibility report warns when a ReadMethod is not valid for the dashboard's vehicle.
- Removed the nonexistent `Kline` ReadMethod. Added the missing ECU Connect names (`DCAN_BMW_*`, `CAN_Ford_Stream`, `KLine_*`) to the fallback list, `KLine_SSM_stream` to Subaru, and `CAN_Ford_Stream` to Lincoln.

## Revision 2.1.2 - 2026-05-23

### Affected Areas

- ReadMethod defaults for new widgets and EDF parsing.
- Per-vehicle ReadMethod resolution and autocomplete ordering.
- BMW and modern Nissan vehicle compatibility.

### Fixes and Changes

- Changed the default `ReadMethod` for new widgets from the legacy `CAN_OBD_LID_Nissan` to `CAN_OBD_RR2_Nissan_Gen1_OEM`, matching the value ECU Connect now writes for VR30TT Gen1b dashboards.
- Added a per-`VehicleId` primary ReadMethod map so VR30TT Gen1/Gen2, RZ34, GTR, 350Z/370Z, Juke, Frontier, Sentra, VK56VD, and all BMW vehicles receive an appropriate default instead of falling through to a generic Nissan literal.
- Populated the previously empty BMW ReadMethod family with `DCAN_BMW_PID`, `DCAN_BMW_CID`, `DCAN_BMW_RR1`, and `CAN_OBD_CID_8205`.
- Reordered the Nissan ReadMethod family so modern `CAN_OBD_RR2_Nissan_Gen1_OEM`, `CAN_OBD_RR2_Nissan_Gen1_RR`, `CAN_OBD_RR2_Nissan_Gen2_OEM`, and `CAN_OBD_RR2_Nissan_Gen2_RR` appear before the legacy LID names in the autocomplete dropdown.
- Added `CAN_Ford_Stream` to the Ford ReadMethod family.
- Updated parser and serializer fallbacks so dashboards saved or loaded with a blank `ReadMethod` field now use the modern Gen1b OEM default instead of the legacy Nissan name.

## Revision 2.1.1 - 2026-05-14

### Affected Areas

- EDF compatibility and saved ZIP/XML output.
- Resolution, canvas fit mode, and widget scaling.
- Widget editor controls, thresholds, and unit handling.
- Right sidebar layout and parameter picker controls.
- Vehicle, parameter, and ECU validation.
- User workflow, compatibility reporting, and documentation.

### Fixes and Changes

- Matched ECU Connect's supported ECU enum values: `Ecm`, `Tcm`, `Abs`, `Tpms`, and `Aux`.
- Removed invalid fallback ECU values such as `Bcm` and `Tcu`.
- Corrected ECU mask handling for `Abs`, `Tpms`, and `Aux`.
- Normalized EDF ZIP entry metadata to match known working EDF files.
- Fixed high-resolution landscape dashboard sizing by using saved physical-pixel dimensions instead of the editor-window size.
- Added physical-pixel dashboard size presets for common phone and tablet layouts.
- Fixed large-dashboard editing so Fit preview mode does not change the saved EDF resolution or widget coordinates.
- Fixed widget drag/resize math while the canvas is displayed in Fit mode.
- Fixed widget preview scaling so text, values, units, bars, charts, LEDs, and buttons scale from each widget's saved size.
- Fixed property-panel checkboxes that could collapse and make `Show Peak`, threshold `Enabled`, and `Flash Screen` controls appear missing.
- Fixed right-sidebar parameter picker and chart channel buttons so they stay visible inside the panel.
- Matched ECU Connect dashboard editor behavior by hiding `Unit` and threshold controls for `Text Digital` and `LED` widgets while preserving their XML fields.
- Matched ECU Connect threshold behavior by showing `Flash Screen` only for enabled `Alarm` thresholds.
- Prevented disabled thresholds from retaining stale `FlashScreen` values.
- Treated blank threshold XML values as disabled instead of `0`.
- Preferred `mph`, `bar`, and `°C` when a selected parameter supports those display units.
- Scaled widget min/max defaults when converting compatible speed, pressure, or temperature units.
- Updated saved EDF output so compatible speed and pressure displays use the preferred units by default.
- Added a pre-save compatibility report for vehicle ID, orientation, bounds, widget size, parameters, ECU types, chart channels, and thresholds.
- Added a post-save summary with vehicle, resolution, page count, widget count, and warnings.
- Added a resolution resize wizard that can either change the saved canvas only or scale all widgets into the new canvas.
- Added a searchable vehicle-aware parameter picker with ECU and parameter-type filters.
- Added visible Undo and Redo toolbar buttons.
- Added multi-select layout tools for alignment, distribution, and widget ordering.
- Added original non-branded logo artwork and renamed the public HTML file to match the project name.
- Added explicit threshold `Enabled` checkboxes.
- Renamed the editor label `ShowPeak` to `Show Peak` while preserving the EDF field name.
- Verified saved EDF structure against known working dashboard files.
- Updated the README/manual with the current editor workflow and app compatibility notes.
