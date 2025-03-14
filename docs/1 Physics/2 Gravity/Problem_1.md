# Problem 1 



# Orbital Period and Orbital Radius

## 1. Theoretical Foundation

Kepler’s Third Law ($T^2 \propto r^3$) is a gem of celestial mechanics. Let’s derive it for circular orbits with clarity and precision, ensuring every step shines.

### Derivation of Kepler’s Third Law

Imagine a small body (mass $m$) orbiting a massive central body (mass $M$), like a planet around the Sun or the Moon around Earth. The orbit is circular with radius $r$. We’ll use Newton’s laws and gravity to connect the orbital period $T$ to the radius $r$.

#### Step 1: Forces at Play
The gravitational pull keeps the body in its circular path:
- **Gravitational force:** The attraction between $m$ and $M$ is given by Newton’s law of gravitation:
  $$
  F_g = \frac{G M m}{r^2}
  $$
  where $G$ is the gravitational constant ($6.67430 \times 10^{-11} \, \text{m}^3 \text{kg}^{-1} \text{s}^{-2}$).

- **Centripetal force:** For circular motion, a force is needed to keep $m$ moving at speed $v$ along the curve:
  $$
  F_c = \frac{m v^2}{r}
  $$

Since gravity provides this centripetal force, we equate them:
$$
\frac{G M m}{r^2} = \frac{m v^2}{r}
$$

#### Step 2: Simplify the Equation
Notice $m$ (the orbiting body’s mass) appears on both sides. As long as $m \neq 0$, we can cancel it:
$$
\frac{G M}{r^2} = \frac{v^2}{r}
$$
Now, multiply both sides by $r$ to clear the denominator on the right:
$$
\frac{G M}{r^2} \cdot r = \frac{v^2}{r} \cdot r
$$
This simplifies to:
$$
\frac{G M}{r} = v^2
$$
So, the orbital velocity squared depends on the central mass and radius:
$$
v^2 = \frac{G M}{r}
$$

#### Step 3: Link Velocity to Period

In a circular orbit, the body travels the circumference ($2\pi r$) in one period ($T$). Thus, the orbital speed is:
$$
v = \frac{\text{Circumference}}{\text{Time}} = \frac{2\pi r}{T}
$$
Square this expression:
$$
v^2 = \left(\frac{2\pi r}{T}\right)^2 = \frac{4\pi^2 r^2}{T^2}
$$


#### Step 4: Combine and Solve


Substitute $v^2 = \frac{4\pi^2 r^2}{T^2}$ into our force equation:
$$
\frac{4\pi^2 r^2}{T^2} = \frac{G M}{r}
$$
To isolate $T^2$, multiply both sides by $T^2$:
$$
4\pi^2 r^2 = \frac{G M}{r} \cdot T^2
$$
Now, divide both sides by $\frac{G M}{r}$ (or multiply by its reciprocal):
$$
T^2 = \frac{4\pi^2 r^2}{\frac{G M}{r}} = \frac{4\pi^2 r^2 \cdot r}{G M}
$$
Simplify the exponents:
$$
T^2 = \frac{4\pi^2 r^3}{G M}
$$
Define the constant $k = \frac{4\pi^2}{G M}$, giving us the final form:
$$
T^2 = k r^3
$$

#### Conclusion
This elegant result shows $T^2$ is directly proportional to $r^3$, with $k$ depending only on $G$ and $M$. It’s universal for circular orbits around the same central mass!

## 2. Implications for Astronomy

- **Mass Calculation:** Measure $r$ and $T$, then solve for $M$:
  $$
  M = \frac{4\pi^2 r^3}{G T^2}
  $$
  Perfect for finding the mass of stars or planets using their moons or satellites.

- **Distance Estimation:** If $M$ is known (e.g., the Sun’s mass), calculate $r$ from $T$.
- **Scalability:** Applies to Solar System planets, exoplanets, and artificial satellites.

## 3. Real-World Examples

- **Moon’s Orbit:**
  - $r = 384,400 \, \text{km} = 3.844 \times 10^8 \, \text{m}$

  - $T = 27.32 \, \text{days} = 2.36 \times 10^6 \, \text{s}$

  - Plug in: $M_{\text{Earth}} = \frac{4\pi^2 (3.844 \times 10^8)^3}{(6.67430 \times 10^{-11}) (2.36 \times 10^6)^2} \approx 5.97 \times 10^{24} \, \text{kg}$.

- **Earth’s Orbit:**
  - $r = 1 \, \text{AU} = 1.496 \times 10^{11} \, \text{m}$

  - $T = 365.25 \, \text{days} = 3.156 \times 10^7 \, \text{s}$

  - Confirms the law with $M_{\text{Sun}} \approx 1.989 \times 10^{30} \, \text{kg}$.

## 4. Implementation

Here’s a polished Python script:

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11
M_sun = 1.989e30  # kg
M_earth = 5.972e24  # kg

# Orbital period (in days)
def orbital_period(r, M):
    return np.sqrt((4 * np.pi**2 * r**3) / (G * M)) / (24 * 3600)

# Data
r_values = np.logspace(7, 11, 100)  # m
T_sun = orbital_period(r_values, M_sun)
T_earth = orbital_period(r_values, M_earth)

# T^2 vs r^3 Plot
plt.figure(figsize=(10, 6))
plt.loglog(r_values**3, T_sun**2, 'b-', label='Around Sun')
plt.loglog(r_values**3, T_earth**2, 'r-', label='Around Earth')
plt.xlabel(r'$r^3$ (m$^3$)')
plt.ylabel(r'$T^2$ (days$^2$)')
plt.title('Kepler’s Third Law')
plt.legend()
plt.grid(True, ls='--')
plt.show()

# Circular Orbit
theta = np.linspace(0, 2 * np.pi, 100)
r_moon = 3.844e8  # m
x, y = r_moon * np.cos(theta), r_moon * np.sin(theta)

plt.figure(figsize=(6, 6))
plt.plot(x, y, 'b-', label='Moon’s Orbit')
plt.plot(0, 0, 'ro', label='Earth')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Moon’s Circular Orbit')
plt.legend()
plt.axis('equal')
plt.grid(True)
plt.show()

# Verification
T_moon = orbital_period(r_moon, M_earth)
print(f"Calculated Moon period: {T_moon:.2f} days (Actual: 27.32)")
```

### Outputs

- **Graph:** Log-log plot shows a perfect line, proving $T^2 \propto r^3$.

- **Orbit:** Visualizes the Moon’s path.
- **Check:** Calculated $T$ matches reality.

## Discussion

- **Elliptical Orbits:** Use semi-major axis $a$: $T^2 = \frac{4\pi^2}{G M} a^3$.

- **Limits:** Assumes $m \ll M$ and no perturbations.
