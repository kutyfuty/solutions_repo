# Problem 1
 
# Investigating the Range as a Function of the Angle of Projection

## 1. Theoretical Foundation

### 1.1 Derivation of Governing Equations

We start with Newton's second law: **F = ma**. In projectile motion, neglecting air resistance, the only force is gravity: **F = -mgj**, where **j** is the unit vector in the upward direction.

Thus, **ma = -mgj** and **a = -gj**. Since acceleration is the second derivative of position:

* d²x/dt² = 0 (horizontal acceleration is zero)
* d²y/dt² = -g (vertical acceleration is -g)

Integrating once gives the velocity:

* dx/dt = v_x0 (horizontal velocity is constant)
* dy/dt = -gt + v_y0 (vertical velocity decreases linearly with time)

Integrating again gives the position:

* x(t) = v_x0 * t + x_0
* y(t) = -1/2 * gt² + v_y0 * t + y_0

If we start at the origin (0, 0) and the initial velocity is v0 at an angle θ:

* v_x0 = v0 * cos(θ)
* v_y0 = v0 * sin(θ)

Therefore:

* x(t) = v0 * cos(θ) * t
* y(t) = -1/2 * gt² + v0 * sin(θ) * t

### 1.2 Family of Solutions

These equations show that the trajectory is a parabola. Changing v0 and θ will result in different trajectories.

## 2. Analysis of the Range

### 2.1 Horizontal Range

To find the range, we set y(t) = 0:

* 0 = -1/2 * gt² + v0 * sin(θ) * t
* t = 2 * v0 * sin(θ) / g (time of flight)

Substituting this time into the x(t) equation gives the range:

* Range (R) = (v0² / g) * sin(2θ)

### 2.2 Influence of Parameters

* **Initial velocity (v0):** The range is proportional to v0².
* **Gravitational acceleration (g):** The range is inversely proportional to g.
* **Angle of projection (θ):** The range is proportional to sin(2θ). The maximum range occurs at θ = 45°.

## 3. Practical Applications

* **Sports:** Trajectories of balls (baseball, golf, soccer).
* **Engineering:** Projectiles, rockets.
* **Astrophysics:** Trajectories of celestial bodies.
* **Uneven Terrain:** Adjust y = 0 to a function of x.
* **Air Resistance:** Add a drag force proportional to velocity.
* **Wind:** Add a wind velocity vector to the horizontal velocity.

## 4. Implementation

```python
import numpy as np
import matplotlib.pyplot as plt

def projectile_range(v0, theta_degrees, g=9.81):
    theta_radians = np.radians(theta_degrees)
    range_val = (v0**2 / g) * np.sin(2 * theta_radians)
    return range_val

def projectile_trajectory(v0, theta_degrees, g=9.81, t_max = 10):
    theta_radians = np.radians(theta_degrees)
    vx0 = v0 * np.cos(theta_radians)
    vy0 = v0 * np.sin(theta_radians)
    t = np.linspace(0, t_max, 100)
    x = vx0 * t
    y = vy0 * t - 0.5 * g * t**2
    return x, y

def plot_range_vs_angle(v0, g=9.81):
    angles = np.linspace(0, 90, 100)
    ranges = [projectile_range(v0, angle, g) for angle in angles]
    plt.plot(angles, ranges)
    plt.xlabel("Angle of Projection (degrees)")
    plt.ylabel("Range (meters)")
    plt.title(f"Range vs. Angle (v0={v0} m/s, g={g} m/s²)")
    plt.grid(True)
    plt.show()

def plot_trajectories(v0, angles, g=9.81):
    plt.figure()
    for angle in angles:
        x, y = projectile_trajectory(v0, angle, g)
        plt.plot(x, y, label=f"Angle: {angle}°")
    plt.xlabel("Horizontal Distance (meters)")
    plt.ylabel("Vertical Distance (meters)")
    plt.title(f"Projectile Trajectories (v0={v0} m/s, g={g} m/s²)")
    plt.legend()
    plt.grid(True)
    plt.ylim(bottom=0)
    plt.show()

# Example usage:
v0 = 20
plot_range_vs_angle(v0)
plot_trajectories(v0, [30, 45, 60])

v0 = 30
plot_range_vs_angle(v0)
plot_trajectories(v0, [30, 45, 60])

g = 1.62 #Moon gravity
plot_range_vs_angle(v0, g)