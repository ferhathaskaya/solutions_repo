# Problem 1
# Simulating the Effects of the Lorentz Force

---
## Motivation

The Lorentz force describes how electric and magnetic fields affect charged particles. It plays a key role in technologies like **mass spectrometers**, **cyclotrons**, and **fusion devices**.



The Lorentz force is given by:

$$
\vec{F} = q\vec{E} + q\vec{v} \times \vec{B}
$$

Where:
- \( \vec{F} \) is the total force acting on the particle,
- \( q \) is the particle's electric charge,
- \( \vec{E} \) is the electric field,
- \( \vec{B} \) is the magnetic field,
- \( \vec{v} \) is the velocity of the particle.

---

## Applications of the Lorentz Force

| System | Role of the Lorentz Force |
|--------|----------------------------|
| **Cyclotrons / Synchrotrons** | Circular motion induced by magnetic fields accelerates particles. |
| **Mass Spectrometers** | Different ions curve based on \( q/m \), enabling identification. |
| **Plasma Confinement** | Magnetic fields trap charged particles in fusion devices. |
| **Auroras & Solar Wind** | Charged particles spiral along Earth's magnetic field lines. |

The ability to control charged particles using \( \vec{E} \) and \( \vec{B} \) fields is foundational in both scientific instrumentation and industrial technology.

---

## Theoretical Derivation

### Uniform Magnetic Field \( \vec{B} = B\hat{z} \), No Electric Field

For a charged particle moving in the \( xy \)-plane with velocity \( \vec{v} = v_x\hat{x} + v_y\hat{y} \), the magnetic part of the Lorentz force is:

$$
\vec{F} = q(\vec{v} \times \vec{B}) = q
\begin{vmatrix}
\hat{x} & \hat{y} & \hat{z} \\
v_x & v_y & 0 \\
0 & 0 & B
\end{vmatrix}
= qB(v_y\hat{x} - v_x\hat{y})
$$

This leads to coupled differential equations:

$$
m\frac{dv_x}{dt} = qBv_y \\
m\frac{dv_y}{dt} = -qBv_x
$$

Taking a time derivative of the first and substituting:

$$
\frac{d^2v_x}{dt^2} = -\left( \frac{qB}{m} \right)^2 v_x
$$

Which is the equation of **simple harmonic motion**, meaning the particle undergoes **circular motion** at the **cyclotron frequency**:

$$
\omega_c = \frac{qB}{m}
$$

The **radius** of the motion is the **Larmor radius**:

$$
r_L = \frac{mv_\perp}{|q|B}
$$

Where \( v_\perp \) is the velocity component perpendicular to the field.

---
## Lorentz Force in a Uniform Magnetic Field

Let’s consider a charged particle moving in a uniform magnetic field \( \vec{B} = B\hat{z} \), with no electric field.

The Lorentz force becomes:

$$
\vec{F} = q(\vec{v} \times \vec{B})
$$

If the particle’s velocity is in the \( xy \)-plane:

$$
\vec{v} = v_x \hat{x} + v_y \hat{y}
$$

Then:

$$
\vec{F} = qB(v_y \hat{x} - v_x \hat{y})
$$

This leads to two coupled equations:

$$
m\frac{dv_x}{dt} = qBv_y \\
m\frac{dv_y}{dt} = -qBv_x
$$

These describe **circular motion** at the **cyclotron frequency**:

$$
\omega_c = \frac{qB}{m}
$$

And the radius of the circular path is the **Larmor radius**:

$$
r_L = \frac{mv_\perp}{|q|B}
$$

Where \( v_\perp \) is the component of velocity perpendicular to the magnetic field.

---

In this configuration, the particle traces a circle (or a helix if it has a velocity component along \( \vec{B} \)).

### Simulation 1: Motion in a Uniform Magnetic Field

<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Parameters
q = 1.0        # Charge (C)
m = 1.0        # Mass (kg)
B = 1.0        # Magnetic field strength (T)
v0 = np.array([1.0, 0.0, 0.0])  # Initial velocity (m/s)
r0 = np.array([0.0, 0.0, 0.0])  # Initial position (m)

# Time settings
dt = 0.01
steps = 1000

# Initialize arrays
positions = np.zeros((steps, 3))
velocities = np.zeros((steps, 3))
positions[0] = r0
velocities[0] = v0

# Magnetic field vector (along z-axis)
B_vec = np.array([0, 0, B])

# Euler method integration
for i in range(steps - 1):
    v = velocities[i]
    F = q * np.cross(v, B_vec)
    a = F / m
    velocities[i + 1] = v + a * dt
    positions[i + 1] = positions[i] + velocities[i + 1] * dt

# Plotting trajectory in xy-plane
plt.figure(figsize=(6, 6))
plt.plot(positions[:, 0], positions[:, 1], label='Particle Path')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Charged Particle in Uniform Magnetic Field')
plt.axis('equal')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()


</code></pre>

</details>

---
![alt text](<Charged Particle in Uniform Magnetic Field.png>)

> **Fig. :** A charged particle moves in a circular path in the $xy$-plane under a uniform magnetic field along the $z$-axis. The Lorentz force acts perpendicular to velocity, producing uniform circular motion.

---
### Simulation 2: Motion in Combined Electric and Magnetic Fields

<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Parameters
q = 1.0        # Charge (C)
m = 1.0        # Mass (kg)
E = np.array([0.5, 0.0, 0.0])  # Electric field (V/m)
B = np.array([0.0, 0.0, 1.0])  # Magnetic field (T)
v0 = np.array([1.0, 0.0, 1.0])  # Initial velocity (m/s)
r0 = np.array([0.0, 0.0, 0.0])  # Initial position (m)

# Time settings
dt = 0.01
steps = 1500

# Initialize arrays
positions = np.zeros((steps, 3))
velocities = np.zeros((steps, 3))
positions[0] = r0
velocities[0] = v0

# Euler method integration with E and B
for i in range(steps - 1):
    v = velocities[i]
    F = q * (E + np.cross(v, B))
    a = F / m
    velocities[i + 1] = v + a * dt
    positions[i + 1] = positions[i] + velocities[i + 1] * dt

# 3D Plot
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')
ax.plot3D(positions[:, 0], positions[:, 1], positions[:, 2], label='Particle Path')
ax.set_xlabel('x (m)')
ax.set_ylabel('y (m)')
ax.set_zlabel('z (m)')
ax.set_title('Charged Particle in Combined E and B Fields')
ax.legend()
plt.tight_layout()
plt.show()



</code></pre>

</details>

---
![alt text](<Charged Particle in Combined E and B Fields.png>)

> **Fig. :** The particle follows a spiral trajectory due to the magnetic field while being steadily accelerated in the direction of the electric field. This results in a helical path that drifts along the 𝑥 x-axis.

---

### Simulation 3 – Crossed Electric and Magnetic Fields (E × B Drift)
<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Parameters
q = 1.0       # Charge (C)
m = 1.0       # Mass (kg)
E = np.array([1.0, 0.0, 0.0])  # Electric field (V/m)
B = np.array([0.0, 0.0, 1.0])  # Magnetic field (T)
v0 = np.array([0.0, 0.0, 0.0])  # Initial velocity (m/s)
r0 = np.array([0.0, 0.0, 0.0])  # Initial position (m)

# Time settings
dt = 0.01
steps = 1500

# Initialize arrays
positions = np.zeros((steps, 3))
velocities = np.zeros((steps, 3))
positions[0] = r0
velocities[0] = v0

# Euler method
for i in range(steps - 1):
    v = velocities[i]
    F = q * (E + np.cross(v, B))
    a = F / m
    velocities[i + 1] = v + a * dt
    positions[i + 1] = positions[i] + velocities[i + 1] * dt

# Plot in xy-plane
plt.figure(figsize=(7, 6))
plt.plot(positions[:, 0], positions[:, 1], label='Particle Path')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Charged Particle in Crossed E and B Fields (E × B Drift)')
plt.grid(True)
plt.axis('equal')
plt.legend()
plt.tight_layout()
plt.show()



</code></pre>

</details>

---

![alt text](<Charged Particle in Crossed E and B Fields (E × B Drift).png>)

> **Fig. :** In crossed electric and magnetic fields, the particle follows a curved path but drifts steadily perpendicular to both fields. This **E × B drift** occurs at a constant velocity $\vec{v}_{\text{drift}} = \frac{\vec{E} \times \vec{B}}{B^2}$. 

---
### Parameter Sweep 1: Varying Magnetic Field Strength 𝐵
<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Constants
q = 1.0
m = 1.0
E = np.array([1.0, 0.0, 0.0])
B_values = [0.5, 1.0, 2.0]
v0 = np.array([0.0, 0.0, 0.0])
r0 = np.array([0.0, 0.0, 0.0])

dt = 0.01
steps = 1500

fig, ax = plt.subplots(figsize=(7, 6))

for B_mag in B_values:
    positions = np.zeros((steps, 3))
    velocities = np.zeros((steps, 3))
    positions[0] = r0
    velocities[0] = v0
    B = np.array([0, 0, B_mag])

    for i in range(steps - 1):
        v = velocities[i]
        F = q * (E + np.cross(v, B))
        a = F / m
        velocities[i + 1] = v + a * dt
        positions[i + 1] = positions[i] + velocities[i + 1] * dt

    ax.plot(positions[:, 0], positions[:, 1], label=f'B = {B_mag} T')

ax.set_xlabel('x (m)')
ax.set_ylabel('y (m)')
ax.set_title('Effect of Magnetic Field Strength on E × B Drift')
ax.grid(True)
ax.axis('equal')
ax.legend()
plt.tight_layout()
plt.savefig("exb_drift_B_variation.png")



</code></pre>

</details>

---

![alt text](<Effect of Magnetic Field Strength on E × B Drift.png>)

> **Fig. :** Particle paths under different magnetic field strengths. Stronger B results in tighter curvature and faster E × B drift alignment. Weaker fields produce broader arcs and slower net motion.

---
### Parameter Sweep 2: Varying Electric Field Strength E
<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Constants
q = 1.0
m = 1.0
B = np.array([0.0, 0.0, 1.0])
E_values = [0.5, 1.0, 2.0]
v0 = np.array([0.0, 0.0, 0.0])
r0 = np.array([0.0, 0.0, 0.0])
dt = 0.01
steps = 1500

fig, ax = plt.subplots(figsize=(7, 6))

for E_mag in E_values:
    E = np.array([E_mag, 0.0, 0.0])
    positions = np.zeros((steps, 3))
    velocities = np.zeros((steps, 3))
    positions[0] = r0
    velocities[0] = v0

    for i in range(steps - 1):
        v = velocities[i]
        F = q * (E + np.cross(v, B))
        a = F / m
        velocities[i + 1] = v + a * dt
        positions[i + 1] = positions[i] + velocities[i + 1] * dt

    ax.plot(positions[:, 0], positions[:, 1], label=f'E = {E_mag} V/m')

ax.set_xlabel('x (m)')
ax.set_ylabel('y (m)')
ax.set_title('Effect of Electric Field Strength on E × B Drift')
ax.grid(True)
ax.axis('equal')
ax.legend()
plt.tight_layout()
plt.savefig("exb_drift_E_variation.png")


</code></pre>

</details>

---

![alt text](<Effect of Electric Field Strength on E × B Drift.png>)

> **Fig. :** Particle trajectories for different electric field strengths. Higher 𝐸 leads to faster E × B drift and wider loop spacing, while 𝐵 remains fixed.

---

### Parameter Sweep 3: Initial Velocity
<details>
<summary>Click to expand full simulation code</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Constants
q = 1.0
m = 1.0
E = np.array([1.0, 0.0, 0.0])
B = np.array([0.0, 0.0, 1.0])
v0_values = [np.array([0.0, 0.0, 0.0]), 
             np.array([0.5, 0.5, 0.0]), 
             np.array([1.0, 0.0, 1.0])]
r0 = np.array([0.0, 0.0, 0.0])
dt = 0.01
steps = 1500

fig, ax = plt.subplots(figsize=(7, 6))

for v0 in v0_values:
    positions = np.zeros((steps, 3))
    velocities = np.zeros((steps, 3))
    positions[0] = r0
    velocities[0] = v0

    for j in range(steps - 1):
        v = velocities[j]
        F = q * (E + np.cross(v, B))
        a = F / m
        velocities[j + 1] = v + a * dt
        positions[j + 1] = positions[j] + velocities[j + 1] * dt

    label = f'v₀ = [{v0[0]}, {v0[1]}, {v0[2]}] m/s'
    ax.plot(positions[:, 0], positions[:, 1], label=label)

ax.set_xlabel('x (m)')
ax.set_ylabel('y (m)')
ax.set_title('Effect of Initial Velocity on E × B Drift')
ax.grid(True)
ax.axis('equal')
ax.legend()
plt.tight_layout()
plt.savefig("exb_drift_v0_variation.png")


</code></pre>

</details>

---

![alt text](<Effect of Initial Velocity on E × B Drift.png>)

> **Fig. :** Changing the initial velocity affects the particle's curvature and orientation but not the net drift direction, which remains governed by $\vec{E} \times \vec{B}$.

## Conclusion

This study demonstrated how charged particles move under electric and magnetic fields, following predictable paths like circles, helices, or drifts. By changing field strengths and initial velocity, we saw how these factors shape the motion. The results confirm the Lorentz force as a key principle in understanding and controlling particle behavior in practical systems.

