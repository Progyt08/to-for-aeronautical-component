# Topology Optimization Framework for Solids of Revolution

Code and data of the undergraduate thesis *Topological Structural Optimization Framework for Solids of Revolution Applied to an Aeronautical Component*, Engineering Physics, EAFIT University, 2026. Author: Fernando Londoño Palacio. Advisor: Ph.D. Ronald Mauricio Martinod Restrepo.

The framework finds the lightest layout of an axisymmetric part made by Fused Filament Fabrication (FFF), keeping its displacements within the drawing tolerances and its von Mises stress within a safety factor of 1.5 under four static load cases. It combines:

- an axisymmetric SIMP implementation in Python, with an axisymmetric Helmholtz filter and a volume constraint weighted by the volume of revolution of each element;
- a transversely isotropic material model, with moduli measured on printed coupons of PA6 and PA6-CF;
- a minimum-mass search on the volume fraction, with displacement and stress limits checked on each binary design;
- closed STL export of rings and test sectors, a virtual compression test and an independent verification.

The full formulation, assumptions and discussion are in the thesis: [`docs/TO_Final_Inform_FLP.pdf`](docs/TO_Final_Inform_FLP.pdf).

## Main results

Calculated masses of the selected designs, for the assumed loads and supports. All of them are lightest feasible designs found, not proven global optima.

| Design | PA6 | Saving | PA6-CF | Saving |
|---|---:|---:|---:|---:|
| Original solid | 1500.7 g | | 1408.4 g | |
| **Multi-load** (satisfies the four cases) | **964.7 g** | **35.7 %** | **537.0 g** | **61.9 %** |
| C1, axial compression | 635.9 g | 57.6 % | 455.2 g | 67.7 % |
| C2, internal pressure | 533.0 g | 64.5 % | 465.0 g | 67.0 % |
| C3, combined | 684.7 g | 54.4 % | 438.4 g | 68.9 % |
| C4, radial pressure | 692.2 g | 53.9 % | 483.0 g | 65.7 % |

Each single-case design violates the limits of the other three cases, so the component needs the multi-load formulation.

Nine full-scale 7.5° sectors were tested in compression. For PA6, the tests preserved the predicted stiffness order of the five designs but measured a mean of 3.54 times the predicted stiffness. For PA6-CF, the predicted order was not preserved, and moisture in the filament prevented printing the multi-load sector. The absolute stiffness and the PA6-CF design ranking therefore remain unvalidated (Sections 5.13 and 6 of the thesis).

## Repository structure

```text
to-for-aeronautical-component/
├── notebooks/
│   ├── material_characterization.ipynb   Coupon tests and material constants (Sec. 4.3, 4.4)
│   ├── Final.ipynb                       The framework, in ten blocks (Chapter 3, Sec. 5.1 to 5.12)
│   └── experimental_compression.ipynb    Tests of the printed sectors (Sec. 4.7, 5.13)
├── data/
│   ├── geometry/Solido_half.stl          CAD of one 180-degree half of the component
│   ├── characterization/                 20 raw records: {PA6,PA6CF}_{1..5}_{T,C}.txt
│   └── sector_tests/                     9 raw records: PA6TO_*.csv, PA6CFTO_*.csv
├── results/
│   ├── report_check.csv                  Results of Final.ipynb against the values printed in the thesis
│   ├── characterization/                 Specimen and batch tables, material constants, figures
│   ├── benchmark/                        Blocks 3 and 4
│   ├── designs/                          Blocks 5 and 6: binary designs, history of the runs, log
│   ├── searches/                         Blocks 5 and 6: figures
│   ├── stl/sectors/                      Block 7: the 7.5-degree sectors, ready to print
│   ├── virtual_test/                     Block 8: curves and tables
│   ├── verification/                     Block 9: tables T01 to T09, figures F01 and F02
│   ├── thesis_figures/                   Block 10: figures F10_xx, tables T10_xx
│   └── sector_tests/                     Figures F_EXP_xx, tables T_EXP_xx
├── docs/
│   ├── TO_Final_Inform_FLP.pdf           The thesis
│   ├── report_traceability.md            Where each figure and table of the thesis comes from
│   ├── component_cad.png
│   └── photos/                           Printer, specimens, test setup and tested sectors
├── requirements.txt
└── LICENSE
```

The 360-degree rings (249 MB) and the benchmark STL files are not stored in git. `Final.ipynb` writes them to `results/stl/rings/` and `results/stl/benchmark/`.

## How the notebooks connect

```text
data/characterization/ --> material_characterization.ipynb --> constants of Table 4.4.1
                                                                    |
data/geometry/Solido_half.stl ------------------------------> Final.ipynb --> designs, STL files, predictions
                                                                                     |
data/sector_tests/ ------------------------------------> experimental_compression.ipynb --> measured vs. predicted
```

The blocks of `Final.ipynb` share their state in memory and must run in order:

| Block | Purpose | Thesis |
|---|---|---|
| 1 | Material constants, meridian section, Q4 voxelization | Sec. 3.2, 3.3.1, 4.4, 4.5 |
| 2 | Constitutive matrix, supports and baseline finite element analysis | Sec. 3.3.2, 4.1.1 |
| 3 | SIMP engine and benchmark | Sec. 3.5, 5.3 |
| 4 | Contours, closed STL revolution and benchmark figures | Sec. 3.7, 5.3 |
| 5 | Multi-load problem and search on the volume fraction | Sec. 3.6, 4.1, 5.5, 5.6 |
| 6 | Single-case problems and cross-utilization | Sec. 3.6, 5.5, 5.8 |
| 7 | Export of the selected designs as rings and 7.5° sectors | Sec. 3.7, 4.7.1 |
| 8 | Virtual compression test between rigid platens | Sec. 3.8, 5.11, 5.12 |
| 9 | Independent verification | Sec. 3.9, 5.1, 5.2, 5.9, 5.10 |
| 10 | Thesis figures and tables, and check against the thesis | Sec. 3.9, Chapter 5 |

## Getting started

The code was run with Python 3.11 on Windows.

```bash
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements.txt
jupyter lab notebooks/
```

### Reproducing the results

By default `Final.ipynb` loads the stored designs and virtual tests, and everything else runs in about 15 minutes. To regenerate them from scratch, change the flags of its first code cell:

```python
RUN_SEARCHES = True        # The 103 SIMP runs of Blocks 5 and 6
RUN_VIRTUAL_TEST = True    # The 12 virtual tests of Block 8
```

| Step | Thesis | Run stored here |
|---|---:|---:|
| Searches of Block 5 | 36.2 min | 20.5 min |
| Searches of Block 6 | 150.7 min | 130.7 min |
| Virtual tests of Block 8 | 22.4 min | 15.0 min |

These are recorded times on the computers used, not a hardware-independent benchmark.

`pypardiso` provides the Intel PARDISO solver and is the solver used for the stored results. Without it, the notebook falls back to SuperLU, which is slower. The problem is non-convex and each run starts from the previous one, so round-off differences between solvers or library versions could change the search path. The last cell of `Final.ipynb` compares each run with the thesis and writes the outcome to `results/report_check.csv`.

### What the stored results contain

| Folder | Produced by | Contents |
|---|---|---|
| `results/characterization/` | `material_characterization.ipynb` | Stress-strain figures, specimen and batch tables, material constants |
| `results/benchmark/` | Blocks 3 and 4 | Fixed-volume benchmark: figures and metrics |
| `results/designs/` | Blocks 5 and 6 | `designs.npz` with the binary design of all 103 runs, `search_history.json` with their metrics, `search_log.txt` with the log of the run |
| `results/searches/` | Blocks 5 and 6 | Mass against utilization for each search, cross-utilization |
| `results/stl/sectors/` | Block 7 | The ten closed 7.5° sectors, named `{material}_{design}_vf{volume fraction}_sec7p5.stl` |
| `results/virtual_test/` | Block 8 | Force-displacement curve and figure of the twelve virtual tests, summary table |
| `results/verification/` | Block 9 | Tables T01 to T09, figures F01 and F02 |
| `results/thesis_figures/` | Block 10 | Figures F10_xx by topic, tables T10_xx as CSV, Markdown and LaTeX, and `figure_captions.md` |
| `results/sector_tests/` | `experimental_compression.ipynb` | Figures F_EXP_xx, tables T_EXP_xx, Table 5.13.1 |
| `results/report_check.csv` | Block 10 | Each compared value: computed, printed in the thesis, and whether they agree |

[`docs/report_traceability.md`](docs/report_traceability.md) maps each figure and table of the thesis to the notebook and file that produce it.

### How the stored run was verified

All the searches and virtual tests were run again from scratch with this consolidated code, and the results were compared with the run behind the thesis:

- `results/report_check.csv` agrees with the thesis in all 115 compared values.
- Tables T01 to T05 and T07 to T10 match the original run, except for the computing times. The original run also has an `E_z alternative` set in T06, from an earlier version of the material catalog, which the thesis does not use and this code does not compute. The other 48 rows of T06 match.
- The ten sector STL files have the same number of triangles and the same volume as the originals.
- Block 9 re-evaluates all the design and load case pairs independently, and differs from the values stored by the searches by at most 7.85e-11 in relative terms.

These checks show that the code reproduces the numbers of the thesis. They do not validate the assumed loads and supports or the material model, whose limitations are discussed in Sections 1.7, 4.8 and 5.13 of the thesis.

### Applying the framework to another part

The formulation is general for a solid of revolution made of a transversely isotropic material under axisymmetric supports and loads. These inputs are specific to this component:

| Input | Where |
|---|---|
| Geometry | `STL_FILE`, in the first code cell. The reorientation rule of Section 1.2 assumes this part's dimensions |
| Material constants | `MATERIAL_CATALOG`, Block 1 |
| Mesh | `NELR`, `NELZ`, Block 1 |
| Supports | `nodes_sliding`, `nodes_clamped`, Block 2 |
| Loaded surfaces and pressures | `top_face`, `cavity_*`, `bore_wall`, `P_NOM`, Block 5 |
| Admissible displacements and safety factor | `U_ADM_CRIT`, `U_ADM_REM`, `CRIT_ZONE_*`, `FS_VM`, Block 5 |

Blocks 9 and 10 and the final check are written for this component's designs, and their reference values would need to be replaced.

## Data notes

- **Thesis values prevail.** The model uses the constants of Table 4.4.1. The current reduction of the coupon tests gives compressive yield strengths of 53.30 and 25.80 MPa, against the 53.29 and 25.84 MPa adopted in the thesis, which come from an earlier iteration of the reduction. The difference does not exceed 0.15 %, and no selected design is governed by stress.
- **Yield strength of PA6 in compression.** Three of the five prisms have no crossing with the 0.2 % offset line, and the reduction uses the 80th percentile of their stress record. `material_characterization.ipynb` identifies them.
- **Apparent density.** The individual masses of the prisms are not in the repository, so `material_characterization.ipynb` takes the averages reported in the thesis.
- **Sector masses.** The printed sectors were not weighed. Their masses come from the STL volume and the apparent density.
- **Repeatability.** There is one record per sector design, so the statistics between designs do not estimate the repeatability between specimens.

The assumptions behind the results, such as the assumed loads and supports and the elastic and homogenized material model, are listed in Sections 1.7 and 4.8 of the thesis.

## Citation

```bibtex
@thesis{londono2026topology,
  author = {Londo{\~n}o Palacio, Fernando},
  title  = {Topological Structural Optimization Framework for Solids of Revolution Applied to an Aeronautical Component},
  school = {EAFIT University},
  type   = {Undergraduate thesis},
  year   = {2026},
  url    = {https://github.com/Progyt08/to-for-aeronautical-component}
}
```

## License

Released under the MIT License. See [`LICENSE`](LICENSE).
