---
{"dg-publish":true,"permalink":"/personal/pavlov3-d/co-pilot-multiple-axis-plot-scaling-algorithm/","noteIcon":"","created":"2025-05-20T10:31:48.785-05:00"}
---

## Query:
> Please recommend a Python code design for a multiple axis plot scaling algorithm. The min and max vlaues in three dimensions should be aligned for every curve, such that the origins represent the average of the min and max for all three-dimensions. Every curve come from a separate three-column CSV, and the code should be generalized for modular vectors, such that they can be sourced from GPX file imports or other filetype imports. Spatially each curve should take of the same amount of space, each if they have different orders of magnitude.

## Answer:

That sounds like a fascinating project! To accomplish this, we can use the `matplotlib` and `pandas` libraries in Python. We can create a function that reads multiple CSV files, normalizes the data, and plots it in three dimensions, ensuring that the min and max values in each dimension are aligned and scaled correctly.

Here’s a Python code design for your requirement:

```python
import pandas as pd
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

def read_csv(file_path):
    return pd.read_csv(file_path)

def normalize_data(df):
    normalized_df = df.copy()
    for col in df.columns:
        normalized_df[col] = (df[col] - df[col].min()) / (df[col].max() - df[col].min())
    return normalized_df

def plot_3d_curves(file_paths):
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    
    all_data = []
    for file_path in file_paths:
        data = read_csv(file_path)
        data = normalize_data(data)
        all_data.append(data)
    
    for data in all_data:
        ax.plot(data.iloc[:, 0], data.iloc[:, 1], data.iloc[:, 2])

    min_vals = [data.min().min() for data in all_data]
    max_vals = [data.max().max() for data in all_data]

    avg_min = sum(min_vals) / len(min_vals)
    avg_max = sum(max_vals) / len(max_vals)

    ax.set_xlim([avg_min, avg_max])
    ax.set_ylim([avg_min, avg_max])
    ax.set_zlim([avg_min, avg_max])
    
    plt.show()

# Example usage
file_paths = ['curve1.csv', 'curve2.csv', 'curve3.csv']
plot_3d_curves(file_paths)
```

This script:
1. Reads multiple CSV files.
2. Normalizes the data from each file.
3. Plots the data on a 3D plot with aligned and scaled axes.

Ensure that each CSV file has three columns representing the three dimensions. You can extend this code to handle other file formats (e.g., GPX) by adding appropriate file reading functions.

Would you like to dive deeper into any specific part of this code? 🚀

## Yes.

Great! Let's dive deeper into the code:

1. **Reading CSV Files:**
   ```python
   def read_csv(file_path):
       return pd.read_csv(file_path)
   ```
   This function reads a CSV file and returns a DataFrame. It's simple and can be easily extended to read other file types by adding more conditions.

2. **Normalizing Data:**
   ```python
   def normalize_data(df):
       normalized_df = df.copy()
       for col in df.columns:
           normalized_df[col] = (df[col] - df[col].min()) / (df[col].max() - df[col].min())
       return normalized_df
   ```
   This function normalizes each column of the DataFrame to a range of 0 to 1. This ensures that all dimensions are on the same scale, which is essential for meaningful visual comparisons.

3. **Plotting 3D Curves:**
   ```python
   def plot_3d_curves(file_paths):
       fig = plt.figure()
       ax = fig.add_subplot(111, projection='3d')
       
       all_data = []
       for file_path in file_paths:
           data = read_csv(file_path)
           data = normalize_data(data)
           all_data.append(data)
       
       for data in all_data:
           ax.plot(data.iloc[:, 0], data.iloc[:, 1], data.iloc[:, 2])
   ```
   This part initializes a 3D plot and reads, normalizes, and stores data from each file path. Each normalized dataset is plotted as a curve in 3D space.

4. **Setting Axis Limits:**
   ```python
   min_vals = [data.min().min() for data in all_data]
   max_vals = [data.max().max() for data in all_data]

   avg_min = sum(min_vals) / len(min_vals)
   avg_max = sum(max_vals) / len(max_vals)

   ax.set_xlim([avg_min, avg_max])
   ax.set_ylim([avg_min, avg_max])
   ax.set_zlim([avg_min, avg_max])
   ```
   This calculates the average of the min and max values across all datasets and uses these averages to set the axis limits, ensuring that all curves are scaled appropriately.

Now, here are a few enhancements you might consider:

- **Handling Different File Types:** Extend the `read_csv` function to read GPX or other file formats.
- **Dynamic Scaling:** Adjust the normalization to handle datasets with different orders of magnitude more effectively.

Would you like to see an example of how to extend the code to handle GPX files or any other specific enhancements? 📈

## Yes.

Perfect! Let's enhance the code to handle GPX files and make it more modular.

Firstly, we need to install a library to read GPX files. We'll use the `gpxpy` library, which can be installed via pip:
```bash
pip install gpxpy
```

Now, let's modify our code to handle GPX files along with CSV files:

```python
import pandas as pd
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import gpxpy
import gpxpy.gpx

def read_csv(file_path):
    return pd.read_csv(file_path)

def read_gpx(file_path):
    with open(file_path, 'r') as gpx_file:
        gpx = gpxpy.parse(gpx_file)
    
    data = {'latitude': [], 'longitude': [], 'elevation': []}
    for track in gpx.tracks:
        for segment in track.segments:
            for point in segment.points:
                data['latitude'].append(point.latitude)
                data['longitude'].append(point.longitude)
                data['elevation'].append(point.elevation)
    
    return pd.DataFrame(data)

def normalize_data(df):
    normalized_df = df.copy()
    for col in df.columns:
        normalized_df[col] = (df[col] - df[col].min()) / (df[col].max() - df[col].min())
    return normalized_df

def plot_3d_curves(file_paths, file_types):
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    
    all_data = []
    for file_path, file_type in zip(file_paths, file_types):
        if file_type == 'csv':
            data = read_csv(file_path)
        elif file_type == 'gpx':
            data = read_gpx(file_path)
        else:
            raise ValueError(f"Unsupported file type: {file_type}")
        
        data = normalize_data(data)
        all_data.append(data)
    
    for data in all_data:
        ax.plot(data.iloc[:, 0], data.iloc[:, 1], data.iloc[:, 2])

    min_vals = [data.min().min() for data in all_data]
    max_vals = [data.max().max() for data in all_data]

    avg_min = sum(min_vals) / len(min_vals)
    avg_max = sum(max_vals) / len(max_vals)

    ax.set_xlim([avg_min, avg_max])
    ax.set_ylim([avg_min, avg_max])
    ax.set_zlim([avg_min, avg_max])
    
    plt.show()

# Example usage
file_paths = ['curve1.csv', 'curve2.gpx', 'curve3.csv']
file_types = ['csv', 'gpx', 'csv']
plot_3d_curves(file_paths, file_types)
```

This updated script includes:

1. **Reading GPX Files:**
   ```python
   def read_gpx(file_path):
       with open(file_path, 'r') as gpx_file:
           gpx = gpxpy.parse(gpx_file)
       
       data = {'latitude': [], 'longitude': [], 'elevation': []}
       for track in gpx.tracks:
           for segment in track.segments:
               for point in segment.points:
                   data['latitude'].append(point.latitude)
                   data['longitude'].append(point.longitude)
                   data['elevation'].append(point.elevation)
       
       return pd.DataFrame(data)
   ```

2. **Handling Multiple File Types:**
   ```python
   def plot_3d_curves(file_paths, file_types):
       fig = plt.figure()
       ax = fig.add_subplot(111, projection='3d')
       
       all_data = []
       for file_path, file_type in zip(file_paths, file_types):
           if file_type == 'csv':
               data = read_csv(file_path)
           elif file_type == 'gpx':
               data = read_gpx(file_path)
           else:
               raise ValueError(f"Unsupported file type: {file_type}")
           
           data = normalize_data(data)
           all_data.append(data)
   ```

This code now handles both CSV and GPX files. You can extend it further for other file types if needed.

What do you think? Any other features or customizations you'd like to add? 📊

Next: [[Personal/Pavlov3D/Co-Pilot Halfwidth Cube Plot Scaling Algorithm\|Co-Pilot Halfwidth Cube Plot Scaling Algorithm]]