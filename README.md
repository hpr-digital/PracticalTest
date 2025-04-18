# Sonar-Vision Technical Assessment

## Overview

This technical assessment evaluates your ability to work with underwater sensor data and apply computer vision techniques. The assessment consists of two main parts:

1. **Sonar Data Visualisation**: Process and visualise mechanical scanning sonar data
2. **Depth Estimation from Images**: Apply depth estimation models to underwater imagery

This assessment is designed to test your skills with data processing, visualisation, and application of existing computer vision models in the context of underwater sensing.

## Assessment Structure

### Part 1: Sonar Data Visualisation (50%)

In this part, you will work with raw sonar data. The data is provided in CSV format and contains mechanical scanning sonar measurements.

**Your tasks:**

1. Parse and understand the structure of the sonar data
2. Create a meaningful visualisation of the sonar data (e.g., polar plot, point cloud)
3. Document your interpretation of what the visualisation reveals about the environment
4. Briefly explain your approach to processing and visualising the data

**Sonar Specifications:**
- Mechanical Scanning Profiling Sonar
- 1° Acoustic Angular Resolution
- 80 Metre Range Capability
- 0.35mm Timing Accuracy
- 360° scanning capability

### Part 2: Depth Estimation from Images (50%)

In this part, you will work with underwater images captured by a underwater camera mounted on an ROV.

**Your tasks:**

1. Select and apply an appropriate depth estimation model to the provided underwater images
2. Visualise the depth estimation results
3. Discuss the challenges and limitations of depth estimation in underwater environments
4. Briefly explain your choice of model and any adaptations you made for underwater imagery

*You might want to use google colab for this. 

## Deliverables
**Presentation** (15 minutes):
   - Prepare a short presentation of your work
   - Include key visualisations and findings
   - Be prepared to explain your technical approach
   - Discuss potential improvements or extensions

## Resources

- `sonar_data/sonar_data.csv`: Raw sonar data in CSV format
- `test_images/`: Folder containing underwater images for depth estimation

## Evaluation Criteria

You will be evaluated on:

1. **Technical Understanding**:
   - Correct interpretation of the sonar data structure
   - Appropriate choice and application of depth estimation model

2. **Implementation Quality**:
   - Code quality, structure, and documentation
   - Effectiveness of visualisations

3. **Analysis and Insights**:
   - Interpretation of results
   - Understanding of limitations and challenges

4. **Communication**:
   - Clarity of report and presentation
   - Ability to explain technical concepts

## Presentation

You will have 15 minutes to present your work, followed by 5 minutes for questions. Your presentation should cover:
- Your approach to both tasks
- Key visualisations and findings
- Challenges encountered and how you addressed them
- Potential improvements with more time

Good luck!
