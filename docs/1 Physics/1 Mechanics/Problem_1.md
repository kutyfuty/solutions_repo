# Problem 1

# Investigating the Range as a Function of the Angle of Projection

## Motivation
Projectile motion offers a rich playground for exploring fundamental principles of physics. The equations governing projectile motion involve both linear and quadratic relationships. This project aims to analyze how the range of a projectile depends on its angle of projection.

## Task

### 1. Theoretical Foundation

#### Governing Equations
Projectile motion can be described using the following basic equations, considering no air resistance and launch from the ground:
- Horizontal motion: \( x(t) = v_0 \cos(\theta) t \)
- Vertical motion: \( y(t) = v_0 \sin(\theta) t - \frac{1}{2} g t^2 \)

#### Range Equation
The horizontal range \( R \) is given by:
\[ R = \frac{v_0^2 \sin(2\theta)}{g} \]

### 2. Analysis of the Range

#### Dependence on Angle
The range depends sinusoidally on the angle \( \theta \), with a maximum at \( \theta = 45^\circ \).

#### Influence of Parameters
- Increasing \( v_0 \) increases the range.
- Increasing \( g \) decreases the range.

### 3. Practical Applications

#### Real-World Adaptations
- On uneven terrain, consider different launch and landing heights.
- Air resistance requires numerical methods for solutions.

### 4. Implementation

Let's create a simple Python script to simulate and visualize the range as a function of the angle of projection:

```python
import numpy as np
import matplotlib.pyplot as plt

# Define the parameters
v0 = 20  # initial velocity in m/s
g = 9.81  # acceleration due to gravity in m/s^2
angles = np.linspace(0, 90, 100)  # angle range from 0 to 90 degrees

# Calculate the range for each angle
ranges = (v0**2 * np.sin(np.radians(2 * angles))) / g

# Plot the results
plt.figure(figsize=(10, 6))
plt.plot(angles, ranges, label='Range vs Angle')
plt.xlabel('Angle of Projection (degrees)', fontsize=14, fontweight='medium')
plt.ylabel('Range (m)', fontsize=14, fontweight='medium')
plt.title('Projectile Range as a Function of Launch Angle', fontsize=16, fontweight='medium')
plt.grid(True)
plt.legend()
plt.show()