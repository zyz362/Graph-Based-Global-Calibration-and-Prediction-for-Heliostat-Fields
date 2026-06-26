# Heliostat Field Calibration and Tracking Error Prediction Dataset

## Overview

This repository contains the heliostat field tracking error dataset collected at the Jülich Solar Center (DLR), along with extended tower measurement data. The data is used for cold-start calibration, tracking error modeling, and Graph Neural Network (GNN) prediction of heliostat fields.

## Data Source

- **Collection Site**: DLR Solar Center, Jülich, Germany
- **Coordinates**: Latitude 50.9136°N, Longitude 6.3875°E, Altitude ~88.7m
- **Tower Height**: ~123m (central receiver tower)
- **Original Reference**: Päumann et al., "High-Accuracy Data-Driven Heliostat Calibration Using a Single-Camera System," *Solar Energy*, 2023.

## Dataset Structure

```
data/
├── paint_data/                  # Heliostat tracking error dataset
│   ├── <HELIOSTAT_ID>/          # Per-heliostat subdirectory
│   │   └── <HELIOSTAT_ID>_properties.json  # Physical & kinematic properties
│   ├── tower/
│   │   └── tower_measurements.json       # Tower measurement station data
│   ├── weather/
│   │   └── 2022-03-juelich-weather.h5    # Weather data (HDF5 format)
│   ├── paint_dataset.csv             # Full tracking error data (24,783 records)
│   ├── paint_dataset_with_weather.csv # Tracking error with weather features (6,057 records)
│   
│
└── pdata/                     # Extended heliostat property dataset
    ├── <HELIOSTAT_ID>/
    │   └── <HELIOSTAT_ID>_properties.json
    └── tower/
        └── tower_measurements.json
```

## Field Descriptions

### paint_dataset.csv / paint_dataset_with_weather.csv

| Field | Description | Unit |
|-------|-------------|------|
| `heliostat_id` | Heliostat ID (e.g., AA31, BE36) | - |
| `timestamp` | Sample timestamp (UTC) | ISO 8601 |
| `sample_id` | Unique sample identifier | - |
| `sun_elevation` | Sun elevation angle | degrees |
| `sun_azimuth` | Sun azimuth angle | degrees |
| `axis_1_motor_position` | Axis 1 motor position (azimuth) | counts |
| `axis_2_motor_position` | Axis 2 motor position (elevation) | counts |
| `target_name` | Target tower name | - |
| `heliostat_lat` | Heliostat latitude | degrees |
| `heliostat_lon` | Heliostat longitude | degrees |
| `heliostat_alt` | Heliostat altitude | meters |
| `focal_spot_lat` | Focal spot latitude | degrees |
| `focal_spot_lon` | Focal spot longitude | degrees |
| `focal_spot_alt` | Focal spot altitude | meters |
| `target_center_lat` | Target center latitude | degrees |
| `target_center_lon` | Target center longitude | degrees |
| `target_center_alt` | Target center altitude | meters |
| `tracking_error_lat` | Tracking error in latitude | degrees |
| `tracking_error_lon` | Tracking error in longitude | degrees |
| `tracking_error_alt` | Tracking error in altitude | meters |
| `wind_speed` | Wind speed | m/s |
| `wind_direction` | Wind direction | degrees |
| `dni` | Direct Normal Irradiance | W/m² |
| `temperature` | Temperature | °C |
| `humidity` | Humidity | % |
| `pressure` | Atmospheric pressure | hPa |

### _properties.json

Each heliostat property file contains:
- `heliostat_position`: [lat, lon, alt] installation position
- `height`, `width`: Mirror dimensions (meters)
- `initial_orientation`: Initial orientation vector
- `kinematics_properties`: Kinematic parameters (actuators, joint stroke, initial angles, movement speed, etc.)

### timestamps.json

Sample timestamp mapping file. Key format: `<heliostat_id>_<sample_id>`, value: UTC timestamp.

### weather/2022-03-juelich-weather.h5

Weather data in HDF5 format, containing meteorological observations for Jülich in March 2022.

## Data Statistics

- **Number of heliostats**: 102+ (covering AC, AB, AK, AA, BA, BE, BF, BG, BH, BI, BJ, BK, BL series)
- **Total records**: 24,783 tracking measurement records
- **Records with weather data**: 6,057

## Data Applications

This dataset is suitable for:
1. **Heliostat cold-start calibration**: Estimate heliostat kinematic parameters without precise initial values
2. **Tracking error modeling**: Learn the relationship between tracking errors and sun position, wind speed, etc.
3. **GNN prediction**: Leverage spatial correlations among heliostats for global error prediction
4. **Meteorological coupling analysis**: Analyze the impact of wind, temperature, and other environmental factors on tracking accuracy

## License

The dataset originates from publicly available research data and is intended for academic research and non-commercial use. When citing, please refer to the original paper:

> Päumann, M., et al. "High-Accuracy Data-Driven Heliostat Calibration Using a Single-Camera System." *Solar Energy*, 2023.

## Version History

- **v1.0** (2026-06-26): Initial release, including paint_data and pdata datasets
