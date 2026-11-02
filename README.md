# Polarimetric SAR Decomposition (geoskill-polarimetric-decomposition)

> Polarimetric SAR decomposition: Cloude-Pottier H/A/α eigen-decomposition and Freeman three-component decomposition, outputting scattering entropy, anisotropy, scattering angle, and surface/dihedral/volume scattering powers.

---

## 1. Overview

Performs polarimetric target decomposition on fully polarimetric SAR data to extract the physical scattering mechanisms of ground targets. Two mainstream methods are implemented:

- **Cloude-Pottier (H/A/α) eigen-decomposition**: applies Hermitian eigen-decomposition to the 3×3 coherency matrix T3 of each pixel, yielding entropy H (scattering randomness, 0 = single mechanism, 1 = fully random), anisotropy A, and scattering angle α (surface scattering α≈0-30°, volume scattering α≈40-60°, dihedral scattering α≈90°).
- **Freeman-Durden three-component decomposition (simplified)**: solves for surface scattering Ps, dihedral scattering Pd, and volume scattering Pv from |Shh|², |Svv|², |Shv|².

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-polarimetric-decomposition.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

- Cloude / H/A/α: input GeoTIFF must contain 9 bands encoding the upper triangle of T3
  `[T11, T22, T33, Re T12, Im T12, Re T13, Im T13, Re T23, Im T23]`.
- Freeman: input GeoTIFF must contain 3 bands `[|Shh|², |Svv|², |Shv|²]`.

| File | Format | Description |
|---|---|---|
| `cloude_H_A_alpha.tif` | GeoTIFF (3 band) | Entropy H, anisotropy A, scattering angle α (degrees) |
| `freeman_three_component.tif` | GeoTIFF (3 band) | Ps / Pd / Pv scattering powers |
| `decomposition_stats.json` | JSON | Statistics and synthetic ground truth |
| `output-manifest.json` | JSON | Run manifest |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
