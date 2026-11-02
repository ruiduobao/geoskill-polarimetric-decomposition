# 极化SAR分解 | Polarimetric SAR Decomposition

对全极化 SAR 数据执行极化目标分解，提取地物的散射物理机制。实现两类主流方法：

- **Cloude-Pottier（H/A/α）特征分解**：对每个像元的 3×3 相干矩阵 T3 做 Hermitian
  特征分解，得到熵 H（散射随机性，0=单一机制、1=完全随机）、各向异性 A 与散射角 α
  （表面散射 α≈0-30°、体散射 α≈40-60°、二面角散射 α≈90°）。
- **Freeman-Durden 三分量分解（简化）**：从 |Shh|²、|Svv|²、|Shv|² 求解表面散射 Ps、
  二面角散射 Pd 与体散射 Pv。

## 应用场景

- 地物分类与散射机制识别（城市建筑、森林、裸土、水体）
- 森林结构参数提取、农作物长势监测
- 极化 SAR 数据的物理特征工程

## 依赖

```bash
pip install 'numpy' 'rasterio' 'scipy'
```

## 输入约定

- Cloude / H/A/α：输入 GeoTIFF 需含 9 个波段，按 T3 上三角编码
  `[T11, T22, T33, Re T12, Im T12, Re T13, Im T13, Re T23, Im T23]`。
- Freeman：输入 GeoTIFF 需含 3 个波段 `[|Shh|², |Svv|², |Shv|²]`。

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-polarimetric-decomposition.py --bbox 116.0 39.0 117.0 40.0 --method cloude --output-dir ./out
```

### 示例 2（Freeman 三分量合成）

```bash
python geoskill-polarimetric-decomposition.py --bbox 116.0 39.0 117.0 40.0 --method freeman --synthetic --output-dir ./out
```

### 示例 3（真实 T3 数据）

```bash
python geoskill-polarimetric-decomposition.py --input T3_9band.tif --method ha_alpha --output-dir ./out
```

### 示例 4（真实 C3 三分量）

```bash
python geoskill-polarimetric-decomposition.py --input C3_3band.tif --method freeman --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `cloude_H_A_alpha.tif` | GeoTIFF (3 band) | 熵 H、各向异性 A、散射角 α（度） |
| `freeman_three_component.tif` | GeoTIFF (3 band) | Ps / Pd / Pv 散射功率 |
| `decomposition_stats.json` | JSON | 统计量与合成真值 |
| `output-manifest.json` | JSON | 运行清单 |

## 数据源 / Source

本地极化 GeoTIFF（T3 9 波段或 C3 3 波段），或 `--synthetic` 物理模拟场景。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
