# thunderscope-lite Project Notes

## Current status

- `thunderscope-lite` is a KiCad 9 project at the repo root.
- Custom/project KiCad libraries have been vendored into `thunderscope-lite/libraries/`.
- Local library tables now point the existing schematic library names at the vendored files:
  - `sym-lib-table`
  - `fp-lib-table`
- Standard KiCad libraries such as `Device`, `power`, `Connector`, `Diode`, `Resistor_SMD`, `Capacitor_SMD`, and `Package_TO_SOT_SMD` are still expected from the installed KiCad library set.

## Vendored libraries

- Symbols are in `libraries/symbols/`.
- Footprints are in `libraries/footprints/`.
- Available custom 3D models are in `libraries/3dmodels/`.
- `AD9288BSTZ-40` and `ADA4930-2YCPZ-R2` footprints reference local STEP files through `${KIPRJMOD}`.
- `731000105.zip` and `UD2-4.5NUN-L.zip` do not contain STEP or WRL models, so no 3D models were available for those parts.

## Notes

- The project keeps existing library names like `AD9288BSTZ-40`, `ADA4930-2YCPZ-R2`, `UD2-4.5NUN-L`, `731000105`, and `Thunderscope_Rev5`.
- The `Thunderscope_Rev5` footprint library now contains the full Rev5.3 project footprint set, currently 61 `.kicad_mod` files.
- KiCad lock file `~thunderscope-lite.kicad_sch.lck` existed before this library work and was left alone.
- KiCad ERC was run on 2026-07-07 and completed without missing-library errors. It reported 108 existing schematic ERC violations, mostly unconnected pins and power pins not driven.
- While this was being edited, KiCad appears to have generated/updated autosave files and a backup zip. These were left untouched.

## 2026-07-08 update

- Copied the full Rev5.3 project footprint library from `ThunderScope/Hardware/Thunderscope_Rev5.3/Thunderscope_Rev5.3.pretty/` into `libraries/footprints/Thunderscope_Rev5.pretty/`.
- Refreshed `libraries/symbols/Thunderscope_Rev5.kicad_sym` from Rev5.3.
- The lite project still uses the library name `Thunderscope_Rev5`; only the local library contents were expanded.

## 2026-07-08 STEP model note

- Rev5.3 does not store its 3D models as loose `.step`, `.stp`, or `.wrl` files in the project tree.
- The Rev5.3 3D models are embedded inside the copied `.kicad_mod` footprint files and referenced with `kicad-embed://...`.
- Fixed the lite copy of `FUJ_FTR-B3SA.kicad_mod` to reference its embedded `FTR-B3SA4.5Z.stp` model instead of the old Windows absolute path.

## 2026-07-08 KiCad library visibility note

- Adding `THUNDER_*` path variables in KiCad Configure Paths does not by itself make symbols or footprints appear in the chooser.
- KiCad loads project libraries from `sym-lib-table` and `fp-lib-table` in the project folder.
- Open `thunderscope-lite.kicad_pro`, then use Preferences -> Manage Symbol Libraries / Manage Footprint Libraries and check the Project Specific tab if vendored libraries do not appear.

## 2026-07-08 embedded model question

- Asked whether the AD9288 and AD4390 STEP files can be embedded into their footprint files.
- AD9288 has a loose STEP file in `libraries/3dmodels/`.
- No AD4390 library has been identified in the project; this may mean `ADA4930-2YCPZ-R2`.
- KiCad 9 footprints can embed 3D model files using `embedded_files` and `kicad-embed://...`, as seen in the copied Rev5.3 footprints.

## 2026-07-08 embedded_files format note

- KiCad 9 footprint embedded models are stored in an `(embedded_files ...)` block inside the `.kicad_mod` file.
- Each embedded file uses `(file (name "...") (type ...) (data |...|) (checksum "..."))`.
- The footprint 3D model entry references the embedded payload with `(model "kicad-embed://filename.step" ...)`.
