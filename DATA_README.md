# 定日镜场标定与跟踪误差预测数据集

## 概述

本仓库包含 Jülich 太阳能研究中心（DLR）公开采集的定日镜场跟踪误差数据集，以及扩展的塔测数据。数据用于定日镜场的冷启动标定、跟踪误差建模与图神经网络（GNN）预测研究。

## 数据来源

- **采集地点**：德国尤利希太阳能研究中心（DLR Solar Center, Jülich, Germany）
- **坐标**：纬度 50.9136°N，经度 6.3875°E，海拔约 88.7m
- **塔高**：约 123m（中央接收塔）
- **采集时间**：2022年3月至2022年7月
- **原始文献**：Päumann et al., "High-Accuracy Data-Driven Heliostat Calibration Using a Single-Camera System," *Solar Energy*, 2023.

## 数据集结构

```
data/
├── paint_data/                  # 定日镜跟踪误差数据集（来自 Pargmann 论文）
│   ├── <HELIOSTAT_ID>/          # 每个子目录对应一个定日镜
│   │   └── <HELIOSTAT_ID>_properties.json  # 定日镜物理属性与运动学参数
│   ├── tower/
│   │   └── tower_measurements.json       # 塔测站位置与测量数据
│   ├── weather/
│   │   └── 2022-03-juelich-weather.h5    # 气象数据（HDF5格式）
│   ├── paint_dataset.csv             # 完整跟踪误差数据（24,783条记录）
│   ├── paint_dataset_with_weather.csv # 含气象特征的跟踪误差数据（6,057条记录）
│   └── timestamps.json               # 样本时间戳映射（24,526个条目）
│
└── pdata/                     # 扩展定日镜属性数据集（来自另一批次采集）
    ├── <HELIOSTAT_ID>/
    │   └── <HELIOSTAT_ID>_properties.json
    └── tower/
        └── tower_measurements.json
```

## 字段说明

### paint_dataset.csv / paint_dataset_with_weather.csv

| 字段 | 说明 | 单位 |
|------|------|------|
| `heliostat_id` | 定日镜编号（如 AA31, BE36） | - |
| `timestamp` | 采样时间戳（UTC） | ISO 8601 |
| `sample_id` | 样本唯一标识 | - |
| `sun_elevation` | 太阳高度角 | 度 |
| `sun_azimuth` | 太阳方位角 | 度 |
| `axis_1_motor_position` | 轴1电机位置（方位轴） | counts |
| `axis_2_motor_position` | 轴2电机位置（俯仰轴） | counts |
| `target_name` | 目标塔名称 | - |
| `heliostat_lat` | 定日镜纬度 | 度 |
| `heliostat_lon` | 定日镜经度 | 度 |
| `heliostat_alt` | 定日镜海拔 | 米 |
| `focal_spot_lat` | 焦点纬度 | 度 |
| `focal_spot_lon` | 焦点经度 | 度 |
| `focal_spot_alt` | 焦点海拔 | 米 |
| `target_center_lat` | 靶心纬度 | 度 |
| `target_center_lon` | 靶心经度 | 度 |
| `target_center_alt` | 靶心海拔 | 米 |
| `tracking_error_lat` | 纬度方向跟踪误差 | 度 |
| `tracking_error_lon` | 经度方向跟踪误差 | 度 |
| `tracking_error_alt` | 海拔方向跟踪误差 | 米 |
| `wind_speed` | 风速 | m/s |
| `wind_direction` | 风向 | 度 |
| `dni` | 直接法向辐照度 | W/m² |
| `temperature` | 温度 | °C |
| `humidity` | 湿度 | % |
| `pressure` | 气压 | hPa |

### _properties.json

每个定日镜的属性文件包含：
- `heliostat_position`: [lat, lon, alt] 安装位置
- `height`, `width`: 镜面尺寸（米）
- `initial_orientation`: 初始朝向向量
- `kinematics_properties`: 运动学参数（ actuators、关节行程、初始角度、运动速度等）

### timestamps.json

样本时间戳映射文件，键格式为 `<heliostat_id>_<sample_id>`，值为 UTC 时间戳。

### weather/2022-03-juelich-weather.h5

气象数据，HDF5 格式，包含 Jülich 地区 2022年3月的气象观测记录。

## 数据统计

- **定日镜数量**：102+ 个（覆盖 AC、AB、AK、AA、BA、BE、BF、BG、BH、BI、BJ、BK、BL 等系列）
- **总记录数**：24,783 条跟踪测量记录
- **含气象数据记录**：6,057 条
- **时间跨度**：2022年3月 - 2022年7月

## 数据用途

本数据集适用于：
1. **定日镜冷启动标定**：无需精确初始参数即可估计定日镜运动学参数
2. **跟踪误差建模**：学习定日镜跟踪误差与太阳位置、风速等因素的关系
3. **图神经网络预测**：利用定日镜间的空间相关性进行全局误差预测
4. **气象耦合分析**：分析风、温度等环境因素对跟踪精度的影响

## 使用许可

数据集来源于学术研究的公开数据，供学术研究与非商业用途使用。引用时请参考原始论文：

> Päumann, M., et al. "High-Accuracy Data-Driven Heliostat Calibration Using a Single-Camera System." *Solar Energy*, 2023.

## 版本历史

- **v1.0** (2026-06-26): 初始发布，包含 paint_data 和 pdata 两个数据集
