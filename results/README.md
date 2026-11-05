# Results

Everything in this folder is produced by the notebooks. [`docs/report_traceability.md`](../docs/report_traceability.md) maps each figure and table of the thesis to its file here.

| Folder | Produced by | Contents |
|---|---|---|
| [`characterization/`](characterization) | `material_characterization.ipynb` | Stress-strain figures, specimen and batch tables, material constants |
| [`benchmark/`](benchmark) | `Final.ipynb`, Blocks 3 and 4 | Fixed-volume benchmark: figures and metrics |
| [`designs/`](designs) | `Final.ipynb`, Blocks 5 and 6 | `designs.npz` with the binary design of all 103 runs, `search_history.json` with their metrics, `search_log.txt` with the log of the run |
| [`searches/`](searches) | `Final.ipynb`, Blocks 5 and 6 | Mass against utilization for each search, cross-utilization |
| [`stl/sectors/`](stl/sectors) | `Final.ipynb`, Block 7 | The ten closed 7.5° sectors, named `{material}_{design}_vf{volume fraction}_sec7p5.stl` |
| [`virtual_test/`](virtual_test) | `Final.ipynb`, Block 8 | Force-displacement curve and figure of the twelve virtual tests, summary table |
| [`verification/`](verification) | `Final.ipynb`, Block 9 | Tables T01 to T09, figures F01 and F02 |
| [`thesis_figures/`](thesis_figures) | `Final.ipynb`, Block 10 | Figures F10_xx by topic, tables T10_xx as CSV, Markdown and LaTeX, and `figure_captions.md` |
| [`sector_tests/`](sector_tests) | `experimental_compression.ipynb` | Figures F_EXP_xx, tables T_EXP_xx, Table 5.13.1 |

Two more files sit at this level or next to the sectors:

- [`report_check.csv`](report_check.csv): each value compared by Block 10, with the computed value, the value printed in the thesis and whether they agree.
- [`stl/stl_exports.csv`](stl/stl_exports.csv): angle, triangles, closure check, volume and mass of each exported STL file.

The 360-degree rings and the benchmark STL files are not stored in git. `Final.ipynb` writes them to `stl/rings/` and `stl/benchmark/`.
