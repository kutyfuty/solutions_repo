# Problem 1

# Investigating the Range as a Function of the Angle of Projection

## 1. Theoretical Foundation

Projectile motion is a classic application of Newton’s laws in two dimensions. Let’s derive the governing equations from first principles, assuming no air resistance for now (we’ll revisit this later).

### Deriving the Equations of Motion

A projectile is launched with an initial velocity $v_0$ at an angle $\theta$ from the horizontal. The acceleration due to gravity $g$ acts downward. We break the motion into horizontal (x) and vertical (y) components:

- **Initial conditions:**

  - Horizontal velocity: $v_{x0} = v_0 \cos\theta$
  - Vertical velocity: $v_{y0} = v_0 \sin\theta$
  - Initial position: $(x_0, y_0) = (0, 0)$ (assuming launch from the origin)


- **Acceleration:**

  - Horizontal: $a_x = 0$
  - Vertical: $a_y = -g$
  

Using the kinematic equations $v = v_0 + at$ and $s = s_0 + v_0 t + \frac{1}{2} a t^2$:

- **Horizontal motion:**
  $$
  x(t) = v_{x0} t = v_0 \cos\theta \cdot t
  $$

- **Vertical motion:**
  $$
  y(t) = v_{y0} t + \frac{1}{2} a_y t^2 = v_0 \sin\theta \cdot t - \frac{1}{2} g t^2
  $$

### Time of Flight

The projectile hits the ground when $y(t) = 0$. Solve:
$$
v_0 \sin\theta \cdot t - \frac{1}{2} g t^2 = 0
$$
Factorize:
$$
t \left( v_0 \sin\theta - \frac{1}{2} g t \right) = 0
$$
Solutions: $t = 0$ (launch) or:
$$
t = \frac{2 v_0 \sin\theta}{g}
$$
This is the time of flight $T$.

### Range Equation

The horizontal range $R$ is the distance traveled when $t = T$:
$$
R = x(T) = v_0 \cos\theta \cdot \frac{2 v_0 \sin\theta}{g}
$$
Using the identity $\sin 2\theta = 2 \sin\theta \cos\theta$:
$$
R = \frac{v_0^2 \sin 2\theta}{g}
$$

### Family of Solutions

This equation reveals a family of solutions parameterized by:
- $v_0$: Initial velocity scales the range quadratically.
- $g$: Gravitational acceleration inversely affects the range.
- $\theta$: The angle determines the sinusoidal variation.

## 2. Analysis of the Range

### Dependence on Angle

The term $\sin 2\theta$ peaks at 1 when $2\theta = 90^\circ$, or $\theta = 45^\circ$, giving the maximum range:
$$
R_{\text{max}} = \frac{v_0^2}{g}
$$
- At $\theta = 0^\circ$ or $90^\circ$, $\sin 2\theta = 0$, so $R = 0$.
- The range is symmetric about $45^\circ$ (e.g., $\theta = 30^\circ$ and $60^\circ$ yield the same range).

### Influence of Parameters

- **Initial Velocity ($v_0$)**: Doubling $v_0$ quadruples $R$, due to the $v_0^2$ term.
- **Gravity ($g$)**: On the Moon ($g \approx 1.62 \, \text{m/s}^2$) versus Earth ($g \approx 9.81 \, \text{m/s}^2$), the range increases significantly for the same $v_0$ and $\theta$.

## 3. Practical Applications

- **Sports**: In soccer or golf, players adjust $\theta$ and $v_0$ to optimize range, though air resistance and spin complicate the ideal $45^\circ$.
- **Engineering**: Artillery design considers terrain and drag, requiring adjusted angles.
- **Astrophysics**: Trajectories on other planets (different $g$) or in space (negligible $g$) adapt this model.

For uneven terrain (launch height $h \neq 0$), the time of flight becomes the positive root of:

$$
h + v_0 \sin\theta \cdot t - \frac{1}{2} g t^2 = 0
$$
This modifies the range, often reducing the optimal angle below $45^\circ$.

## 4. Implementation

Here’s a Python script to simulate and visualize the range versus angle:

```python
import numpy as np
import matplotlib.pyplot as plt

def range_projectile(v0, theta_deg, g=9.81):
    theta = np.radians(theta_deg)
    return (v0**2 * np.sin(2 * theta)) / g

# Parameters
v0_values = [10, 20, 30]  # m/s
g_values = [9.81, 1.62]   # Earth, Moon
theta = np.linspace(0, 90, 91)  # 0 to 90 degrees

# Plotting
plt.figure(figsize=(10, 6))
for v0 in v0_values:
    for g in g_values:
        R = range_projectile(v0, theta, g)
        label = f'v0 = {v0} m/s, g = {g} m/s²'
        plt.plot(theta, R, label=label)

plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (meters)')
plt.title('Range vs. Angle of Projection')
plt.legend()
plt.grid(True)
plt.show()

# Maximum range example
v0 = 20
g = 9.81
theta_max = 45
R_max = range_projectile(v0, theta_max, g)
print(f"Max range at 45° with v0 = {v0} m/s, g = {g} m/s²: {R_max:.2f} m")
```

### Output Explanation

- The plot shows $R$ versus $\theta$ for different $v_0$ and $g$.
- Peaks at $45^\circ$ confirm the theoretical maximum.
- Higher $v_0$ or lower $g$ increases the range, as expected.

## Discussion and Limitations

### Idealized Model

This model assumes:
- No air resistance.
- Flat terrain ($h = 0$).
- Constant $g$.

### Real-World Adjustments

- **Drag**: Introduces a velocity-dependent force, reducing range and shifting the optimal angle (typically < $45^\circ$).

- **Wind**: Adds a horizontal force, altering the trajectory.

- **Numerical Simulation**: For drag, solve the differential equations numerically (e.g., using Runge-Kutta methods) since no closed-form solution exists.

### Suggestions

Incorporate drag with $F_d = -k v$ or terrain effects by adjusting the landing condition. Simulate these using Python libraries like `scipy.integrate.odeint`.
