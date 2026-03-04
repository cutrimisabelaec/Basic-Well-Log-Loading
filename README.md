# Basic Well Log Loading (LAS) - Colab

Python (Google Colab) workflow to **read `.LAS` well log files**, perform **basic preprocessing**, and **plot logs** (histograms + depth tracks). It also computes **Vp (m/s)** from sonic logs (**DT/DTC/DTCO**).

---

## What this code does

- Mounts Google Drive in Colab and accesses a `.las` file
- Reads LAS using `lasio` and converts to a `pandas.DataFrame`
- Creates a depth column `MD` (Measured Depth) from the LAS index
- Converts `MD` to **meters** when values look like **feet**
- If `T2_DIST` / `T2DIST` columns exist:
  - Sums them **row-wise** and stores the result in `NMRTT`
  - Drops the original `T2_DIST` / `T2DIST` columns
- Automatically searches for the first available sonic curve among `DT`, `DTC`, or `DTCO`
  - Computes **Vp** in **m/s**, respecting LAS units (`us/ft`, `us/m`, `s/m`)
  
## Outputs

  Plots:
- Histograms for available curves
- 4 tracks with depth increasing downward:
  - **Vp (m/s)** vs depth
  - **RHOB (g/cm³)** vs depth
  - **NPHI (v/v)** vs depth
  - **GR (API)** vs depth

## Common issues

- DT/DTC/DTCO not found
  - Check the LAS mnemonics (las.curves) and adjust the filter startswith(('DT','DTC','DTCO')) if needed.

- Different DT unit
  - Vp calculation depends on las.curves[dt_column].unit. If it is empty/unexpected, the code assumes **us/ft**.
---

## Requirements

- Python 3
- Libraries:
  - `lasio`
  - `pandas`
  - `numpy`
  - `matplotlib`
