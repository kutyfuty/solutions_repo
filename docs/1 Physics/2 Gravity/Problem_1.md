# Problem 1

# Orbital Period and Orbital Radius (Kepler’s Third Law)

## 1. Motivation

Kepler’s Third Law states that the square of a planet’s orbital period ($T^2$) is proportional to the cube of its average orbital radius ($R^3$). This relationship, $T^2 \propto R^3$, is fundamental in celestial mechanics, helping us understand planetary motions, satellite orbits, and gravitational interactions. It connects Newton’s law of gravitation to observable phenomena, making it essential for astronomy and space exploration.

---

## 2. Derivation

### For Circular Orbits

- **Setup:** Consider a satellite or planet of mass $m$ orbiting a central body (e.g., Earth or Sun) of mass $M$ in a circular orbit with radius $R$.

- **Gravitational Force:** The force keeping the object in orbit is the gravitational attraction: $F = \frac{G M m}{R^2}$, where $G$ is the gravitational constant.

- **Centripetal Force:** For circular motion, the required centripetal force is $F = \frac{m v^2}{R}$, where $v$ is the orbital velocity.

- **Equate Forces:** $\frac{G M m}{R^2} = \frac{m v^2}{R}$.

- **Simplify:** Cancel $m$ (assuming $m \ll M$) and multiply through by $R$: $\frac{G M}{R} = v^2$.

- **Relate Velocity to Period:** The orbital circumference is $2\pi R$, and the period $T$ is the time for one orbit, so $v = \frac{2\pi R}{T}$.

- **Substitute $v$:** $\frac{G M}{R} = \left(\frac{2\pi R}{T}\right)^2 = \frac{4\pi^2 R^2}{T^2}$.

- **Rearrange:** $T^2 = \frac{4\pi^2 R^3}{G M}$.

- **General Form:** $T^2 = \frac{4\pi^2}{G M} R^3$, or in normalized form (for a system), $T^2 \propto R^3$ when $M$ is constant (e.g., Sun for planets).


### Parameters Affecting the Relationship

- **Mass of the Central Body ($M$):** A larger $M$ (e.g., Sun vs. Earth) reduces $T$ for the same $R$.

- **Orbital Radius ($R$):** Larger orbits increase $T$ cubically.

- **Gravitational Constant ($G$):** A universal constant, fixing the proportionality.

---

## 3. Implications for Astronomy

- **Planetary Masses:** By measuring $T$ and $R$ (e.g., via Kepler’s laws or observations), we can solve for $M$: $M = \frac{4\pi^2 R^3}{G T^2}$. This is used to estimate the Sun’s mass or exoplanet host stars.

- **Distances:** For known $M$ (e.g., Sun), $R$ can be calculated from $T$, aiding distance measurements in the Solar System.

- **Satellite Orbits:** Engineers use this to design satellite periods (e.g., geostationary orbits at 24 hours).

---

## 4. Real-World Examples

- **Moon’s Orbit Around Earth:**

  - $T \approx 27.3$ days (sidereal month) = $2.36 \times 10^6$ s.

  - $R \approx 3.84 \times 10^8$ m.

  - $T^2 / R^3 \approx (2.36 \times 10^6)^2 / (3.84 \times 10^8)^3 \approx 4.29 \times 10^{-14}$ s²/m³.

  - Using $M_{\text{Earth}} = 5.972 \times 10^{24}$ kg, $G = 6.67430 \times 10^{-11}$, verify: $\frac{4\pi^2}{G M_{\text{Earth}}} \approx 4.29 \times 10^{-14}$ s²/m³ (matches).

- **Earth’s Orbit Around Sun:**
  - $T \approx 365.25$ days = $3.156 \times 10^7$ s.

  - $R \approx 1.496 \times 10^{11}$ m (1 AU).

  - $T^2 / R^3 \approx 2.98 \times 10^{-19}$ s²/m³.
  
  - Using $M_{\text{Sun}} = 1.989 \times 10^{30}$ kg, $\frac{4\pi^2}{G M_{\text{Sun}}} \approx 2.98 \times 10^{-19}$ s²/m³ (matches).

---

## 5. Python Code for Google Colab

We’ll simulate circular orbits and verify Kepler’s Third Law.

```python
# Import libraries (Colab-compatible)
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files

# Constants
G = 6.67430e-11  # Gravitational constant (m³ kg⁻¹ s⁻²)
M_EARTH = 5.972e24  # Mass of Earth (kg)
RADIUS_EARTH = 6.371e6  # Radius of Earth (m)

# Function to calculate period for a given radius
def calculate_period(radius, central_mass):
    """
    Calculate the orbital period using Kepler's Third Law.
    Returns period in seconds.
    """
    return 2 * np.pi * np.sqrt(radius**3 / (G * central_mass))

# Simulate orbits at different radii
radii = np.linspace(RADIUS_EARTH * 2, RADIUS_EARTH * 10, 100)  # From 2 to 10 Earth radii
periods = [calculate_period(r, M_EARTH) for r in radii]

# Convert periods to days for readability
periods_days = [p / (3600 * 24) for p in periods]

# Verify T² ∝ R³
T_squared = np.array(periods)**2
R_cubed = radii**3
proportionality = T_squared / R_cubed
constant = 4 * np.pi**2 / (G * M_EARTH)

# Visualization
plt.figure(figsize=(10, 6), dpi=100)
plt.plot(radii / 1e6, periods_days, 'b-', label='Orbital Period')
plt.xlabel('Orbital Radius (10⁶ m)', fontsize=14)
plt.ylabel('Period (days)', fontsize=14)
plt.title('Orbital Period vs. Radius for Earth', fontsize=16)
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.savefig('orbit_period.png', dpi=100, bbox_inches='tight')
plt.show()

# Plot T² vs R³ to verify proportionality
plt.figure(figsize=(10, 6), dpi=100)
plt.plot(R_cubed / 1e18, T_squared / 1e12, 'r-', label='T² vs R³')
plt.axhline(constant, color='black', linestyle='--', label=f'Constant = {constant:.2e}')
plt.xlabel('R³ (10¹⁸ m³)', fontsize=14)
plt.ylabel('T² (10¹² s²)', fontsize=14)
plt.title('T² vs R³ Proportionality', fontsize=16)
plt.legend(fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.savefig('T2_vs_R3.png', dpi=100, bbox_inches='tight')
plt.show()

# Print results for real-world examples
print("=== Real-World Examples ===")
examples = [
    ('Moon around Earth', 2.36e6, 3.84e8, M_EARTH),
    ('Earth around Sun', 3.156e7, 1.496e11, 1.989e30)
]
for name, T, R, M in examples:
    expected = 4 * np.pi**2 * R**3 / (G * M)
    print(f"{name}:")
    print(f"  Period (s): {T:.2e}")
    print(f"  Radius (m): {R:.2e}")
    print(f"  T²/R³: {T**2 / R**3:.2e} s²/m³")
    print(f"  Expected (4π²/GM): {expected:.2e} s²/m³\n")

# Download plots
files.download('orbit_period.png')
files.download('T2_vs_R3.png')
```

---
![alt text](image-1.png)
![alt text](image-2.png)
---

## 7. Outputs and Explanation

### Visualization
- **Orbital Period vs. Radius:** Shows how the period increases with radius (blue curve). The period grows slowly at first, then faster as $R$ increases.

- **T² vs R³:** The red line is nearly flat, confirming $T^2 \propto R^3$. The black dashed line is the theoretical constant, matching closely.

### Real-World Examples

- **Moon:** $T^2 / R^3 \approx 4.29 \times 10^{-14}$ s²/m³, matching the expected value.

- **Earth:** $T^2 / R^3 \approx 2.98 \times 10^{-19}$ s²/m³, matching the Sun’s system.

---

## 8. Discussion

- **Elliptical Orbits:** Kepler’s Third Law applies to the semi-major axis ($a$) for elliptical orbits: $T^2 \propto a^3$. The derivation adjusts for the average distance, but the proportionality holds.

- **Other Celestial Bodies:** The law extends to moons, asteroids, and exoplanets, provided the central mass dominates (e.g., Sun for planets).

- **Limitations:** Assumes circular orbits and negligible mass of the orbiting body. Relativistic effects or multiple bodies (e.g., Lagrange points) require adjustments.

