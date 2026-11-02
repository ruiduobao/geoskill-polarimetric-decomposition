---
name: geoskill-polarimetric-decomposition
description: '极化SAR分解：Cloude-Pottier H/A/α 特征分解与 Freeman 三分量分解，输出散射熵、各向异性、散射角与表面/二面角/体散射功率'
---

# 极化SAR分解 | Polarimetric SAR Decomposition

Performs polarimetric target decomposition on fully polarimetric SAR data to extract the physical scattering mechanisms of surface features. Two mainstream methods are implemented:

- **Cloude-Pottier (H/A/α) eigen decomposition**: performs a Hermitian eigen decomposition of the 3×3 coherency matrix T3 for each pixel, yielding the entropy H (scattering randomness; 0 = a single mechanism, 1 = fully random), the anisotropy A and the scattering angle α (surface scattering α≈0–30°, volume scattering α≈40–60°, dihedral scattering α≈90°).
- **Freeman-Durden three-component decomposition (simplified)**: solves for the surface scattering Ps, double-bounce scattering Pd and volume scattering Pv from |Shh|², |Svv|² and |Shv|².

## Application Scenarios / 应用场景

- Land-cover classification and scattering mechanism identification (urban buildings, forest, bare soil, water)
- Extraction of forest structural parameters, crop growth monitoring
- Physical feature engineering for polarimetric SAR data

## Dependencies / 依赖

```bash
pip install 'numpy' 'rasterio' 'scipy'
```

## Input Conventions / 输入约定

- Cloude / H/A/α: the input GeoTIFF must contain 9 bands encoding the upper triangle of T3 as `[T11, T22, T33, Re T12, Im T12, Re T13, Im T13, Re T23, Im T23]`.
- Freeman: the input GeoTIFF must contain 3 bands `[|Shh|², |Svv|², |Shv|²]`.

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-polarimetric-decomposition.py --bbox 116.0 39.0 117.0 40.0 --method cloude --output-dir ./out
```

### Example 2 (Freeman three-component synthetic)

```bash
python geoskill-polarimetric-decomposition.py --bbox 116.0 39.0 117.0 40.0 --method freeman --synthetic --output-dir ./out
```

### Example 3 (real T3 data)

```bash
python geoskill-polarimetric-decomposition.py --input T3_9band.tif --method ha_alpha --output-dir ./out
```

### Example 4 (real C3 three-component)

```bash
python geoskill-polarimetric-decomposition.py --input C3_3band.tif --method freeman --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `cloude_H_A_alpha.tif` | GeoTIFF (3 band) | Entropy H, anisotropy A, scattering angle α (degrees) |
| `freeman_three_component.tif` | GeoTIFF (3 band) | Ps / Pd / Pv scattering powers |
| `decomposition_stats.json` | JSON | Statistics and synthetic ground truth |
| `output-manifest.json` | JSON | Run manifest |

## Data Source / 数据源 / Source

Local polarimetric GeoTIFF (T3 9-band or C3 3-band), or physically simulated scenes via `--synthetic`.

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; user data is never uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
