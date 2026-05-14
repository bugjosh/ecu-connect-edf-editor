# Bugfix Changelog

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
