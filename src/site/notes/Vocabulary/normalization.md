---
{"dg-publish":true,"permalink":"/vocabulary/normalization/","noteIcon":"","created":"2025-02-07T09:51:54.678-06:00"}
---


See: [[Personal/Pavlov3D/Co-Pilot Halfwidth Cube Plot Scaling Algorithm\|Co-Pilot Halfwidth Cube Plot Scaling Algorithm]]
### **Normalization**

**Normalization** is a process used to adjust values measured on different scales to a common scale. It doesn't necessarily involve logarithmic transformation. In general, normalization can mean:

1. **Min-Max Normalization:**
    
    - This scales the data to a fixed range, typically [0, 1]. It's done by subtracting the minimum value and dividing by the range (maximum - minimum).
        
    
    python
    
    ```
    normalized_value = (value - min_value) / (max_value - min_value)
    ```
    
2. **Z-score Normalization (Standardization):**
    
    - This centers the data around the mean (0) and scales it by the standard deviation. This transformation is useful when the data has a Gaussian (bell-shaped) distribution.
        
    
    python
    
    ```
    normalized_value = (value - mean) / standard_deviation
    ```
    
3. **Decimal Scaling:**
    
    - This scales the data by moving the decimal point of values, which is determined by the maximum absolute value in the dataset.
        
    
    python
    
    ```
    normalized_value = value / 10^j
    ```
    
    where jj is the smallest integer such that max(|normalized_value|) < 1.
    
4. **Logarithmic Normalization:**
    
    - This applies a log transformation to compress the range of the data. This is particularly useful for data with large variations.
        
    
    python
    
    ```
    normalized_value = log(value)
    ```
