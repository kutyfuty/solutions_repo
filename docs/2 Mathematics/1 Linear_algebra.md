# Investigating the Range as a Function of the Angle of Projection

## 1. Theoretical Foundation

Projectile motion is governed by Newton’s laws of motion under constant gravitational acceleration, assuming no air resistance for simplicity. Let’s derive the equations step-by-step.

### Derivation of Governing Equations
Consider a projectile launched from the origin $$(x_0, y_0) = (0, 0)$$ with initial velocity $$v_0$$ at an angle $$\theta$$ from the horizontal. The acceleration due to gravity is $$g$$, acting downward.

- **Horizontal motion**: No horizontal acceleration ($$a_x = 0$$).
  $$
  \frac{d^2x}{dt^2} = 0 \quad \Rightarrow \quad \frac{dx}{dt} = v_x = v_0 \cos\theta
  $$
  Integrating:
  $$
  x(t) = (v_0 \cos\theta) t
  $$

- **Vertical motion**: Constant downward acceleration ($$a_y = -g$$).
  $$

  \frac{d^2y}{dt^2} = -g \quad \Rightarrow \quad \frac{dy}{dt} = v_y = v_0 \sin\theta - gt

  $$
  Integrating again:
  $$

  y(t) = (v_0 \sin\theta) t - \frac{1}{2} g t^2
  $$

### Time of Flight
The projectile hits the ground when $$y(t) = 0$$:
$$
(v_0 \sin\theta) t - \frac{1}{2} g t^2 = 0
$$
Factorizing:
$$
t \left( v_0 \sin\theta - \frac{1}{2} g t \right) = 0
$$
Solutions: $$t = 0$$ (launch) or:
$$
t = \frac{2 v_0 \sin\theta}{g}
$$
This is the time of flight, $$T$$.

### Range
The horizontal range $$R$$ is the distance traveled when $$t = T$$:
$$
R = x(T) = (v_0 \cos\theta) \cdot \frac{2 v_0 \sin\theta}{g} = \frac{2 v_0^2 \sin\theta \cos\theta}{g}
$$
Using the identity $$2 \sin\theta \cos\theta = \sin(2\theta)$$:
$$
R = \frac{v_0^2 \sin(2\theta)}{g}
$$
This is the range as a function of the angle of projection $$\theta$$.

### Family of Solutions
The equation $$R = \frac{v_0^2 \sin(2\theta)}{g}$$ defines a family of solutions parameterized by:
- $$v_0$$: Initial velocity.
- $$g$$: Gravitational acceleration.
- Initial height $$h$$ (if $$y_0 \neq 0$$), which modifies the time of flight and range (explored later).

## 2. Analysis of the Range

### Dependence on Angle $$\theta$$
- $$R$$ is maximized when $$\sin(2\theta) = 1$$, i.e., $$2\theta = 90^\circ$$ or $$\theta = 45^\circ$$.
- Maximum range: $$R_{\text{max}} = \frac{v_0^2}{g}$$.
- Symmetry: $$R(\theta) = R(90^\circ - \theta)$$, e.g., ranges at $$30^\circ$$ and $$60^\circ$$ are equal.
- $$R = 0$$ at $$\theta = 0^\circ$$ and $$\theta = 90^\circ$$.

### Influence of Parameters
- **Initial Velocity ($$v_0$$)**: $$R \propto v_0^2$$. Doubling $$v_0$$ quadruples the range.
- **Gravity ($$g$$)**: $$R \propto \frac{1}{g}$$. Lower gravity (e.g., on the Moon) increases range.
- **Initial Height ($$h$$)**: If launched from height $$h$$, solve $$y(t) = h + (v_0 \sin\theta) t - \frac{1}{2} g t^2 = 0$$. This increases the time of flight and thus the range, breaking the $$\theta = 45^\circ$$ optimum.

## 3. Practical Applications

- **Sports**: Optimizing the launch angle in basketball or golf. Air resistance and spin complicate real trajectories.
- **Engineering**: Artillery and rocket launches adjust for terrain and atmospheric effects.
- **Astrophysics**: Trajectories of objects in varying gravitational fields (e.g., planetary motion approximates parabolas locally).
- **Uneven Terrain**: Adjust the landing condition (e.g., $$y = h_{\text{land}}$$) to model slopes.
- **Air Resistance**: Introduce a drag term proportional to velocity, requiring numerical solutions.

## 4. Implementation

Below is a Python script to simulate and visualize the range as a function of $$\theta$$.

```python
import numpy as np
import matplotlib.pyplot as plt

def range_projectile(v0, theta_deg, g=9.81, h=0):
    """Calculate range given initial velocity, angle (degrees), gravity, and height."""
    theta = np.radians(theta_deg)
    if h == 0:
        return (v0**2 * np.sin(2 * theta)) / g
    else:
        # Time of flight with initial height
        a = -g / 2
        b = v0 * np.sin(theta)
        c = h
        t = (-b + np.sqrt(b**2 - 4*a*c)) / (2*a)  # Positive root
        return v0 * np.cos(theta) * t

# Parameters
v0_values = [10, 20, 30]  # m/s
g = 9.81  # m/s^2
theta_deg = np.linspace(0, 90, 91)  # 0 to 90 degrees

# Plot range vs angle for different v0
plt.figure(figsize=(10, 6))
for v0 in v0_values:
    R = [range_projectile(v0, t, g) for t in theta_deg]
    plt.plot(theta_deg, R, label=f'v0 = {v0} m/s')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (m)')
plt.title('Range vs Angle of Projection (h = 0)')
plt.legend()
plt.grid(True)
plt.show()

# Effect of initial height
h_values = [0, 5, 10]  # meters
v0 = 20  # m/s
plt.figure(figsize=(10, 6))
for h in h_values:
    R = [range_projectile(v0, t, g, h) for t in theta_deg]
    plt.plot(theta_deg, R, label=f'h = {h} m')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (m)')
plt.title('Range vs Angle of Projection (v0 = 20 m/s)')
plt.legend()
plt.grid(True)
plt.show()
```

### Output
- **First Plot**: Range vs. $$\theta$$ for $$v_0 = 10, 20, 30 \, \text{m/s}$$, $$h = 0$$. Peaks at 45°.
- **Second Plot**: Range vs. $$\theta$$ for $$h = 0, 5, 10 \, \text{m}$$, $$v_0 = 20 \, \text{m/s}$$. Higher launch points shift the optimal angle below 45°.

## Discussion and Limitations

### Limitations
- **Idealization**: Assumes no air resistance, flat terrain, and constant $$g$$.
- **Range Formula**: Breaks down for $$h \neq 0$$ without adjusting the time of flight.
- **Real-World Factors**: Drag, wind, and spin are ignored.

### Extensions
- **Drag**: Add $$-k v$$ to the differential equations and solve numerically (e.g., using `scipy.integrate.odeint`).
- **Wind**: Include a velocity term in the horizontal equation.
- **Terrain**: Model $$y_{\text{land}}(x)$$ as a function and solve numerically.

This analysis and simulation provide a solid foundation for understanding projectile motion while highlighting its adaptability to complex scenarios.
