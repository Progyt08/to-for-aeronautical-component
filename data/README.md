# Data

Inputs of the notebooks. The test records are stored as the testing machine exported them, so their headers are in Spanish. All of them use `;` as separator, `,` as decimal mark and Latin-1 encoding.

## `geometry/`

`Solido_half.stl` is the CAD of one 180-degree half of the component. `Final.ipynb` reorients it and extracts the meridian section.

## `characterization/`

Twenty records of the printed coupons, read by `material_characterization.ipynb`. They are named `{material}_{specimen}_{test}.txt`:

| Field | Values |
|---|---|
| `material` | `PA6`, `PA6CF` |
| `specimen` | `1` to `5` |
| `test` | `T`, tension: displacement in mm and force in kN |
| | `C`, compression: force in N and displacement in mm, after a table of results from the machine |

## `sector_tests/`

Nine records of the printed 7.5° sectors, with force in N and displacement in mm, read by `experimental_compression.ipynb`:

| File | Material | Design |
|---|---|---|
| `PA6TO_1.csv` | PA6 | Multi-load |
| `PA6TO_2.csv` | PA6 | C4 |
| `PA6TO_3.csv` | PA6 | C3 |
| `PA6TO_4.csv` | PA6 | C2 |
| `PA6TO_5.csv` | PA6 | C1 |
| `PA6CFTO_1.csv` | PA6-CF | C1 |
| `PA6CFTO_2.csv` | PA6-CF | C2 |
| `PA6CFTO_3.csv` | PA6-CF | C3 |
| `PA6CFTO_4.csv` | PA6-CF | C4 |

The PA6-CF multi-load sector could not be printed, because of moisture in the filament.
