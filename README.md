# Portal Method – Lateral Load Analysis

An interactive Jupyter notebook that applies the **Portal Method** to
approximate the shear forces and bending moments in the beams and columns of a
multi-storey, multi-bay rectangular frame subjected to lateral loads (wind /
seismic).

---

## What is the Portal Method?

The Portal Method is a hand-calculation approximation technique used in
structural engineering for the preliminary analysis of building frames under
**lateral (horizontal) loads**.  It assumes:

1. A **point of contra-flexure (zero bending moment) at mid-height** of every column.
2. A **point of contra-flexure at mid-span** of every beam.
3. **Interior columns carry twice the shear** of exterior columns in the same storey.

These assumptions reduce the frame to a set of simple portals that can be solved
using statics alone, making it very fast compared to the cantilever method or a
full finite-element analysis.

---

## Project Structure

```
portal-method/
│
├── data/
│   └── portal.xlsx          ← Input file (edit this with your frame data)
│
├── portal_method.ipynb      ← Main notebook (run this)
│
├── requirements.txt         ← Python dependencies
├── .gitignore               ← Files Git should ignore
└── README.md                ← You are here
```

---

## Input File Format (`data/portal.xlsx`)

Open `data/portal.xlsx` and fill in one **row per storey**, from top to bottom:

| Column name | Unit | Description |
|---|---|---|
| `beams` | — | Storey number (1 = top storey) |
| `force` | kN | Lateral force applied **at this storey level** |
| `distance between beams from top` | m | Storey height (floor-to-floor) |
| `distance between columns` | m | Bay width (column-to-column) |

**Example (3-storey, 4-column frame):**

| beams | force | distance between beams from top | distance between columns |
|---|---|---|---|
| 1 | 15 | 3.5 | 5 |
| 2 | 30 | 3.5 | 7 |
| 3 | 30 | 4.0 | 5 |

> The number of **bays** is inferred from the number of non-empty rows in the
> `distance between columns` column (bays = rows with a value, columns = bays + 1).

---

## Getting Started

### Option A – Run on Google Colab (no installation needed)

1. Go to [colab.research.google.com](https://colab.research.google.com).
2. Click **File → Upload notebook** and upload `portal_method.ipynb`.
3. Upload `portal.xlsx` to the Colab file panel (click the folder icon on the
   left sidebar → upload).
4. In the notebook, update `EXCEL_PATH` to match where you uploaded the file,
   e.g. `"/content/portal.xlsx"`.
5. Click **Runtime → Run all**.

### Option B – Run locally

**Prerequisites:** Python 3.8 or newer.

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/portal-method.git
cd portal-method

# 2. (Recommended) Create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook portal_method.ipynb
```

Then run all cells (`Kernel → Restart & Run All`).

---

## Output

The notebook produces:

- **Printed tables** – column shear, column moment, beam shear, beam moment
  for every storey and bay.
- **Shear Force Diagram** – plotted on the frame outline and saved as
  `shear_force_diagram.png`.
- **Bending Moment Diagram** – plotted on the frame outline and saved as
  `bending_moment_diagram.png`.
- **Summary DataFrames** – final results in a clean tabular format.

---

## Requirements

See `requirements.txt`.  The core dependencies are:

| Package | Purpose |
|---|---|
| `pandas` | Reading the Excel input and building result tables |
| `openpyxl` | Excel (.xlsx) engine for pandas |
| `numpy` | Numerical helpers |
| `matplotlib` | Plotting the SFD and BMD |

---

## Limitations

- The Portal Method is an **approximate** method.  Results may differ from
  those of an exact analysis (e.g. stiffness-matrix / FEM) by 10–30 % depending
  on the frame geometry.
- It is best suited for **regular, rectangular frames** with roughly equal bay
  widths and storey heights.
- The method does **not** account for axial deformations, shear deformations,
  or geometric nonlinearity.

---

## License

MIT License – see `LICENSE` (add one via GitHub when you create the repo).

---

## Author

*Your Name Here* – feel free to open an issue or pull request if you find a bug
or want to suggest an improvement.
