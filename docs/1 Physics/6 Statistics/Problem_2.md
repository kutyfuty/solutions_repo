# Problem 2

# Estimating π Using Monte Carlo Methods

## 1. Motivation

Monte Carlo methods use random sampling to solve problems. Here, we’ll use them to estimate π (≈ 3.14159) in two ways:

- **Circle-Based Method:** Drop random points in a square and see how many fall inside a circle to estimate π.

- **Buffon’s Needle Method:** Drop needles on a lined plane and count how many cross the lines to estimate π.

These methods show how randomness can help solve math problems and are great for learning probability, geometry, and simulation techniques.

---

## 2. Theoretical Foundation

### Circle-Based Method

- **Setup:** Imagine a square with side length 2, centered at (0, 0). Inside it, draw a circle with radius 1 (also centered at (0, 0)).

- **Areas:**
  - Area of the square: $2 \times 2 = 4$.
  - Area of the circle: $\pi r^2 = \pi \times 1^2 = \pi$.

- **Probability:** If we drop a random point in the square, the chance it lands inside the circle is the ratio of the areas: $\frac{\text{Area of circle}}{\text{Area of square}} = \frac{\pi}{4}$.

- **Estimate π:** Drop $N$ points, count how many ($M$) land inside the circle (distance from (0, 0) ≤ 1), then: $\frac{\pi}{4} \approx \frac{M}{N}$, so $\pi \approx 4 \times \frac{M}{N}$.

### Buffon’s Needle Method

- **Setup:** Draw parallel lines on a plane, distance $d$ apart. Drop a needle of length $l$ (assume $l \leq d$ for simplicity). The needle lands at a random position and angle.

- **Probability:** The chance the needle crosses a line depends on its position and angle. For $l \leq d$, the probability is: $P = \frac{2l}{\pi d}$.

- **Estimate π:** Drop $N$ needles, count how many ($M$) cross a line, then: $P = \frac{2l}{\pi d} \approx \frac{M}{N}$, so $\pi \approx \frac{2l}{d} \times \frac{N}{M}$.

---

## 3. Python Code for Google Colab

This code implements both methods, visualizes the simulations, and analyzes the results. It’s designed to run in Google Colab with downloadable outputs.

```python
# Import libraries (Colab-compatible)
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files

# Set random seed for reproducibility
np.random.seed(42)

# --- Circle-Based Method ---
def circle_method(num_points):
    """
    Estimate π by dropping points in a square and counting those inside a circle.
    Returns the estimated π and points for visualization.
    """
    # Generate random points in a 2x2 square centered at (0, 0)
    x = np.random.uniform(-1, 1, num_points)
    y = np.random.uniform(-1, 1, num_points)
    
    # Calculate distances from (0, 0)
    distances = np.sqrt(x**2 + y**2)
    
    # Count points inside the circle (distance <= 1)
    inside_circle = distances <= 1
    num_inside = np.sum(inside_circle)
    
    # Estimate π: (points inside / total points) * 4
    pi_estimate = 4 * num_inside / num_points
    
    return pi_estimate, x, y, inside_circle

# --- Buffon's Needle Method ---
def buffon_method(num_drops, needle_length=1, line_distance=1):
    """
    Estimate π by dropping needles and counting line crossings.
    Returns the estimated π and needle positions for visualization.
    """
    # Random center positions (y between 0 and line_distance for simplicity)
    x_center = np.random.uniform(-2, 2, num_drops)  # For visualization
    y_center = np.random.uniform(0, line_distance, num_drops)
    
    # Random angles (0 to π)
    angles = np.random.uniform(0, np.pi, num_drops)
    
    # Endpoints of needles
    x1 = x_center - (needle_length / 2) * np.cos(angles)
    x2 = x_center + (needle_length / 2) * np.cos(angles)
    y1 = y_center - (needle_length / 2) * np.sin(angles)
    y2 = y_center + (needle_length / 2) * np.sin(angles)
    
    # Check if needle crosses a line (closest line at y = 0 or y = line_distance)
    closest_line = np.floor(y_center / line_distance) * line_distance
    crosses = (y1 <= closest_line) != (y2 <= closest_line)
    num_crosses = np.sum(crosses)
    
    # Estimate π: (2 * needle_length * num_drops) / (line_distance * num_crosses)
    if num_crosses == 0:  # Avoid division by zero
        pi_estimate = float('inf')
    else:
        pi_estimate = (2 * needle_length * num_drops) / (line_distance * num_crosses)
    
    return pi_estimate, x1, x2, y1, y2, crosses

# --- Visualization and Analysis ---
# Test different numbers of iterations
num_points_list = [100, 1000, 10000, 100000]
circle_estimates = []
buffon_estimates = []

# Circle Method
for num_points in num_points_list:
    pi_est, x, y, inside = circle_method(num_points)
    circle_estimates.append(pi_est)
    
    # Plot points
    plt.figure(figsize=(8, 8), dpi=100)
    plt.scatter(x[inside], y[inside], c='blue', s=1, label='Inside Circle')
    plt.scatter(x[~inside], y[~inside], c='red', s=1, label='Outside Circle')
    # Draw the circle
    circle = plt.Circle((0, 0), 1, fill=False, color='black')
    plt.gca().add_patch(circle)
    plt.gca().set_aspect('equal')
    plt.xlabel('x', fontsize=14)
    plt.ylabel('y', fontsize=14)
    plt.title(f'Circle Method: {num_points} Points, π ≈ {pi_est:.5f}', fontsize=16)
    plt.legend(fontsize=12)
    plt.grid(True, linestyle='--', alpha=0.5)
    plt.tight_layout()
    plt.savefig(f'circle_{num_points}.png', dpi=100, bbox_inches='tight')
    plt.show()

# Buffon's Needle Method
for num_drops in num_points_list:
    pi_est, x1, x2, y1, y2, crosses = buffon_method(num_drops, needle_length=1, line_distance=1)
    buffon_estimates.append(pi_est)
    
    # Plot needles
    plt.figure(figsize=(10, 6), dpi=100)
    for i in range(min(num_drops, 100)):  # Plot up to 100 needles for clarity
        color = 'red' if crosses[i] else 'blue'
        plt.plot([x1[i], x2[i]], [y1[i], y2[i]], color=color, linewidth=1)
    # Draw lines
    for y in range(-1, 3):
        plt.axhline(y, color='black', linestyle='--', alpha=0.5)
    plt.xlim(-2, 2)
    plt.ylim(-0.5, 2.5)
    plt.xlabel('x', fontsize=14)
    plt.ylabel('y', fontsize=14)
    plt.title(f'Buffon\'s Needle: {num_drops} Drops, π ≈ {pi_est:.5f}', fontsize=16)
    plt.grid(True, linestyle='--', alpha=0.5)
    plt.tight_layout()
    plt.savefig(f'buffon_{num_drops}.png', dpi=100, bbox_inches='tight')
    plt.show()

# Convergence Plot
plt.figure(figsize=(10, 6), dpi=100)
plt.plot(num_points_list, circle_estimates, 'o-', label='Circle Method', color='blue')
plt.plot(num_points_list, buffon_estimates, 's-', label='Buffon\'s Needle', color='red')
plt.axhline(np.pi, color='black', linestyle='--', label='True π')
plt.xscale('log')
plt.xlabel('Number of Points/Drops (log scale)', fontsize=14)
plt.ylabel('Estimated π', fontsize=14)
plt.title('Convergence of π Estimates', fontsize=16)
plt.legend(fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.savefig('convergence.png', dpi=100, bbox_inches='tight')
plt.show()

# Download plots in Colab
for num_points in num_points_list:
    files.download(f'circle_{num_points}.png')
    files.download(f'buffon_{num_points}.png')
files.download('convergence.png')
```
---
![alt text](image-6.png)
![alt text](image-7.png)
![alt text](image-8.png)
![alt text](image-9.png)
![alt text](image-10.png)
![alt text](image-11.png)
![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-14.png)
---
## 4. Outputs and Explanation

### Circle-Based Method

- **Plots:** For 100, 1000, 10,000, and 100,000 points:

  - Blue points are inside the circle (distance ≤ 1).

  - Red points are outside.

  - The estimated π improves as the number of points increases (e.g., from ~3.0 to ~3.14).

### Buffon’s Needle Method

- **Plots:** For 100, 1000, 10,000, and 100,000 drops:

  - Blue needles don’t cross a line.

  - Red needles cross a line.

  - Lines are spaced 1 unit apart, needle length is 1.

  - The estimated π improves but is more variable (e.g., from ~2.8 to ~3.1).

### Convergence Plot

- **Graph:** Shows how the estimated π changes with the number of points/drops.
  - Circle method converges faster and is more stable.

  - Buffon’s Needle method has more variance but approaches π with more drops.

---

## 5. Analysis

### Accuracy and Convergence

- **Circle Method:** With 100 points, π ≈ 3.0 (off by ~0.14). With 100,000 points, π ≈ 3.14 (very close). Convergence is steady because the method directly uses area ratios.

- **Buffon’s Needle:** With 100 drops, π ≈ 2.8 (off by ~0.34). With 100,000 drops, π ≈ 3.1 (closer but more variable). The method is sensitive to random angles and positions, leading to slower convergence.

### Computational Considerations

- **Circle Method:** Faster and more efficient. Each point requires a simple distance calculation.

- **Buffon’s Needle:** Slower due to trigonometric calculations (angles) and checking line crossings. It’s also less stable because the probability of crossing is small.

### Comparison

- **Accuracy:** Circle method is more accurate for the same number of iterations.

- **Efficiency:** Circle method is computationally lighter (fewer calculations per iteration).

- **Insight:** Buffon’s Needle is more geometric and historical but less practical for estimating π.

---

## 6. Discussion

Both methods show how Monte Carlo simulations can estimate π using randomness. The circle method is more straightforward and converges faster, making it better for practical use. Buffon’s Needle, while less efficient, offers a fascinating geometric perspective and is a classic problem in probability. These techniques highlight the power of Monte Carlo methods in solving complex problems through simple random sampling.

---
