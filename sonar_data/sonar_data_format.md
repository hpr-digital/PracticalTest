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

## Visualisation Approaches

When visualising this sonar data, consider these approaches:

1. **Polar Plot Visualisation**:
   - Use the normalized angle (0-360°) as the theta value
   - Use the calculated range (√(X² + Y² + Z²)) as the radial value
   - This creates a top-down view of the sonar scan
   - Example code structure:
     ```python
     import matplotlib.pyplot as plt
     import numpy as np
     
     # Calculate range from X, Y, Z
     df['range'] = np.sqrt(df['x']**2 + df['y']**2 + df['z']**2)
     
     # Convert angle to radians (after normalizing to 0-360°)
     df['angle_norm'] = (df['angle_raw'] / 10) % 360
     df['angle_rad'] = np.radians(df['angle_norm'])
     
     # Create polar plot
     fig = plt.figure(figsize=(10, 10))
     ax = fig.add_subplot(111, projection='polar')
     ax.scatter(df['angle_rad'], df['range'])
     ax.set_theta_zero_location('N')  # 0° at the top (North)
     ax.set_theta_direction(-1)  # Clockwise direction
     ```

2. **3D Point Cloud Visualisation**:
   - Plot the X, Y, Z values directly in 3D space
   - This shows the actual spatial distribution of detected objects
   - Example code structure:
     ```python
     from mpl_toolkits.mplot3d import Axes3D
     
     fig = plt.figure(figsize=(10, 8))
     ax = fig.add_subplot(111, projection='3d')
     ax.scatter(df['x'], df['y'], df['z'])
     ax.set_xlabel('X (forward)')
     ax.set_ylabel('Y (starboard)')
     ax.set_zlabel('Z (down)')
     ```

3. **Heatmap Visualisation**:
   - Convert the data to a 2D grid based on angle and range
   - Use color intensity to show signal strength
   - Particularly useful for identifying structures and patterns
   - Can be implemented using numpy's histogram2d and matplotlib's imshow

4. **Time Series Analysis**:
   - Group data by timestamp to analyze how the environment changes across multiple scans
   - Plot specific angles over time to track moving objects
   - Use animation to show the scanning process

5. **Combined Visualisations**:
   - Overlay sonar data on a map or reference image
   - Create split views showing different perspectives of the same data
   - Use interactive widgets to explore the data dynamically

## Recommended Libraries

- **Matplotlib**: Good for basic plotting and polar charts
- **Plotly**: Excellent for interactive 3D visualisations
- **Pandas**: For data manipulation and processing
- **NumPy**: For numerical operations and calculations
- **Open3D**: Specialized library for 3D point cloud processing and visualization

Remember to clearly explain your visualization choices and what insights they reveal about the underwater environment.
