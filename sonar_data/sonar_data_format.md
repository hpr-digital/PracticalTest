# Sonar Data Format Guide

## Mechanical Scanning Profiling Sonar Data Format

The provided sonar data file contains measurements from a mechanical scanning profiling sonar that captures a 360° profile of the underwater environment. Each row in the dataset represents a single sonar ping at a specific angle as the sonar head rotates.

### Understanding the Raw Data

The data is provided without the standard NMEA header for simplicity. Here's a sample of the raw data format:

```
2322.002 12552 0.003 0.173 0.047 188.3 2.7 179.4 1482 05/02/2025*2F
2322.002 12544 0.003 0.183 0.055 188.3 2.7 179.4 1482 05/02/2025*24
2322.002 12536 0.003 0.196 0.048 188.3 2.8 179.6 1482 05/02/2025*24
```

In a full NMEA format, these would be preceded by a message identifier that indicates the type of sonar data.

### Detailed Column Descriptions

| Column Position | Field | Description | Unit | Example Value | Notes |
|-----------------|-------|-------------|------|---------------|-------|
| 1 | Timestamp | Time of the ping measurement | seconds | 2322.002 | Seconds from the start of the data collection |
| 2 | Angle | Bearing/angle of the sonar head | 0.1 degrees | 12552 | Must be divided by 10 to get degrees (1255.2°), then use modulo 360 to get standard angle (e.g., 175.2°) |
| 3 | X | X-position of detected target | meters | 0.003 | Forward position relative to the sonar |
| 4 | Y | Y-position of detected target | meters | 0.173 | Starboard (right) position relative to the sonar |
| 5 | Z | Z-position of detected target | meters | 0.047 | Downward position relative to the sonar |
| 6 | Roll | Roll angle of the sonar | degrees | 188.3 | Rotation around the forward axis |
| 7 | Pitch | Pitch angle of the sonar | degrees | 2.7 | Rotation around the lateral axis |
| 8 | Yaw | Heading/yaw of the sonar | degrees | 179.4 | Rotation around the vertical axis |
| 9 | Speed of Sound | Current speed of sound in water | m/s | 1482 | Used for range calculations |
| 10 | Date and Checksum | Date of measurement and data integrity check | - | 05/02/2025*2F | Format: DD/MM/YYYY*XX where XX is a hexadecimal checksum |

### Important Notes for Interpretation

1. **Understanding the Angle Values**: 
   - The raw angle values (column 2) represent the mechanical position of the sonar head
   - Divide by 10 to get the actual angle in degrees (e.g., 12552 ÷ 10 = 1255.2°)
   - Use modulo 360 to normalize to standard bearing (e.g., 1255.2° mod 360 = 175.2°)
   - This represents the direction from which the ping data was collected

2. **Target Position Interpretation**:
   - The X, Y, Z values (columns 3, 4, 5) represent the position of detected targets
   - When these values are 0 or very small, it may indicate no target was detected at that angle
   - Larger values indicate stronger returns, suggesting the presence of objects

3. **Coordinate System**:
   - X-axis: Positive forward (along the heading direction)
   - Y-axis: Positive to starboard (right of the sonar)
   - Z-axis: Positive downward
   - This follows standard ROV coordinate conventions

4. **Range Calculation**: 
   - The direct range to an object can be calculated using the Pythagorean formula:
   - Range = √(X² + Y² + Z²)
   - This gives the straight-line distance from the sonar to the detected object

5. **Reading the Dataset**:
   - Data is collected sequentially as the sonar head rotates
   - A full 360° scan contains 360 pings (at 1° resolution)
   - Consecutive rows with similar timestamps represent a single scan
   - Changes in roll, pitch, and yaw indicate the sonar's orientation was changing during the scan

6. **Understanding Zero Values**:
   - Rows where X, Y, Z are all 0 (e.g., `0 0 0`) typically indicate no return was detected at that angle
   - This could mean either open water (no obstacles) or the signal was too weak to register

Remember to clearly explain your visualization choices (library) and what insights they reveal about the environment.
