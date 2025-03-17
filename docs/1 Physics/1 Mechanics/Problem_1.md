# Problem 1

import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81  # Acceleration due to gravity (m/s^2)
v0_values = [10, 20, 30]  # Different initial velocities

# Angle range
angles = np.linspace(0, 90, 100)  # 0 to 90 degrees
angles_rad = np.radians(angles)  # Convert to radians

plt.figure(figsize=(8, 6))

# Compute and plot range for different initial velocities
for v0 in v0_values:
    R = (v0**2 * np.sin(2 * angles_rad)) / g
    plt.plot(angles, R, label=f'v0 = {v0} m/s')

# Labels and legend
plt.xlabel("Angle of Projection (degrees)")
plt.ylabel("Range (m)")
plt.title("Projectile Range as a Function of Angle")
plt.legend()
plt.grid()
plt.show()
