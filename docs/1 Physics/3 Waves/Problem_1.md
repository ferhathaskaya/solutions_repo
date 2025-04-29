# Problem 1
# Interference Patterns on a Water Surface from Multiple Point Sources

---

Wave interference occurs when waves from different sources overlap, creating new patterns. On a water surface, this is easy to observe: ripples from different points meet, producing regions where the waves either reinforce (constructive interference) or cancel each other out (destructive interference).

This project models and analyzes interference patterns created by a symmetric arrangement of point sources. Through this simulation, we explore the principles of wave superposition and gain a clearer understanding of how coherent waves interact.

---

## Theoretical Model

### Single-Source Circular Wave

A circular wave from a point source located at \((x_0, y_0)\) on the water surface is described by:

$$
\eta(x, y, t) = \frac{A}{\sqrt{r}} \cos(kr - \omega t + \phi)
$$

Where:

- \( \eta(x, y, t) \) = displacement of the water surface at point \((x, y)\) and time \(t\)  
- \( A \) = wave amplitude  
- \( r = \sqrt{(x - x_0)^2 + (y - y_0)^2} \) = distance from source to the point  
- \( k = \frac{2\pi}{\lambda} \) = wave number  
- \( \omega = 2\pi f \) = angular frequency  
- \( \phi \) = initial phase  
- The \( \frac{1}{\sqrt{r}} \) term models radial amplitude decay (spherical spreading in 2D)

###  Superposition of Multiple Waves

For \(N\) sources located at the vertices of a regular polygon, the total displacement at a point \((x, y)\) is the **sum** of the individual wave contributions:

$$
\eta_{\text{total}}(x, y, t) = \sum_{i=1}^{N} \frac{A}{\sqrt{r_i}} \cos(kr_i - \omega t + \phi)
$$

Where \( r_i \) is the distance from the \(i^{\text{th}}\) source to the point \((x, y)\). All sources are assumed to emit **coherent** waves (same frequency and constant phase relationship).

---

##  Simulation Setup (Square Configuration)

For this simulation:
- We place 4 point sources at the **corners of a square** centered at the origin
- All emit waves with the same amplitude, wavelength, and frequency
- We compute the net surface displacement at every point on a grid

###  Parameters Used:
- Amplitude \( A = 1 \)
- Wavelength \( \lambda = 2 \)
- Frequency \( f = 1 \)
- Polygon: square (4 sources)

---

##  Static Visualization at \( t = 0 \)

Interference pattern from square sources
<details>
<summary>Click to expand full simulation code for  interference pattern</summary>

<pre><code>
    # Re-run after kernel reset
import numpy as np
import matplotlib.pyplot as plt

# Wave parameters
A = 1            # Amplitude
wavelength = 2   # Wavelength (lambda)
frequency = 1    # Frequency (f)
phi = 0          # Initial phase
k = 2 * np.pi / wavelength
omega = 2 * np.pi * frequency

# Grid setup
grid_size = 300
x = np.linspace(-5, 5, grid_size)
y = np.linspace(-5, 5, grid_size)
X, Y = np.meshgrid(x, y)

# Square vertex positions (centered)
L = 4  # side length
vertices = [
    (-L/2, -L/2),
    (-L/2,  L/2),
    ( L/2,  L/2),
    ( L/2, -L/2)
]

# Superposition at a fixed time t = 0
t = 0
eta_total = np.zeros_like(X)
for (x0, y0) in vertices:
    R = np.sqrt((X - x0)**2 + (Y - y0)**2) + 1e-6  # Avoid division by zero
    eta = (A / np.sqrt(R)) * np.cos(k * R - omega * t + phi)
    eta_total += eta

# Plotting
plt.figure(figsize=(8, 6))
plt.contourf(X, Y, eta_total, levels=100, cmap='RdBu')
plt.colorbar(label='Displacement η(x, y, t=0)')
plt.title('Interference Pattern from 4 Point Sources (Square)')
plt.xlabel('x')
plt.ylabel('y')
plt.axis('equal')
plt.tight_layout()
plt.show()

</code></pre>

</details>

---
![alt text](<Interference Pattern From 4 Point Sources (Square).png>)


> **Fig. 1:** The water surface displacement due to four coherent wave sources arranged in a square. Bright areas show constructive interference; dark regions show destructive interference.

---

##  Animated Visualization

Wave animation

<details>
<summary>Click to expand full simulation code for animated GIF showing the interference pattern of four wave</summary>

<pre><code>
    import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# --- Parameters ---
A = 1            # Amplitude
wavelength = 2   # Wavelength
frequency = 1    # Frequency
phi = 0          # Initial phase
k = 2 * np.pi / wavelength
omega = 2 * np.pi * frequency

# --- Grid setup ---
grid_size = 300
x = np.linspace(-5, 5, grid_size)
y = np.linspace(-5, 5, grid_size)
X, Y = np.meshgrid(x, y)

# --- Square vertices (sources) ---
L = 4  # Side length of square
vertices = [
    (-L/2, -L/2),
    (-L/2,  L/2),
    ( L/2,  L/2),
    ( L/2, -L/2)
]

# --- Set up the figure ---
fig, ax = plt.subplots(figsize=(8, 6))
cmap = plt.get_cmap('RdBu')

# Initial contour (dummy)
contour = ax.contourf(X, Y, np.zeros_like(X), levels=100, cmap=cmap)
cbar = plt.colorbar(contour, ax=ax)
cbar.set_label('Displacement η(x, y, t)')
ax.set_title('Wave Interference Pattern')
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_aspect('equal')

# --- Update function for animation ---
def update(frame):
    t = frame * 0.1  # Time increment
    eta_total = np.zeros_like(X)
    
    for (x0, y0) in vertices:
        R = np.sqrt((X - x0)**2 + (Y - y0)**2) + 1e-6  # Small term to avoid divide-by-zero
        eta = (A / np.sqrt(R)) * np.cos(k * R - omega * t + phi)
        eta_total += eta
    
    ax.clear()
    contour = ax.contourf(X, Y, eta_total, levels=100, cmap=cmap)
    ax.set_title(f'Wave Interference at t = {t:.1f} s')
    ax.set_xlabel('x')
    ax.set_ylabel('y')
    ax.set_aspect('equal')
    return contour.collections

# --- Create animation ---
ani = animation.FuncAnimation(fig, update, frames=60, blit=False)

# --- Save as GIF ---
ani.save('square_wave_interference.gif', writer='pillow', fps=10)

plt.close(fig)


</code></pre>

</details>

---

![alt text](square_wave_interference.gif)

> **Fig. 2:** Animated view of wave interference over time. Standing wave-like zones emerge where waves reinforce or cancel consistently.

---

##  Interference Pattern Analysis

###  Symmetry

- The square symmetry leads to **rotational and reflectional symmetry** in the interference pattern.
- Patterns radiate outward but maintain consistent nodal (zero-displacement) and antinodal (maximum displacement) structures.

###  Constructive Interference

Occurs where the path difference from multiple sources is an integer multiple of the wavelength:

$$
\Delta r = n\lambda \quad (n \in \mathbb{Z})
$$

At these points, waves arrive in-phase and amplify.

###  Destructive Interference

Occurs when the path difference equals a half-integer multiple of the wavelength:

$$
\Delta r = \left(n + \frac{1}{2}\right)\lambda
$$

Waves arrive out-of-phase and cancel out.

---

##  Effect of Parameters (Discussion)

| Parameter       | Effect on Pattern                          |
|----------------|---------------------------------------------|
| **Wavelength**  | Longer wavelength → broader fringes        |
| **Frequency**   | Affects animation speed (not spatial shape)|
| **Amplitude**   | Affects brightness/intensity only          |
| **Polygon Shape** | More sides → circular symmetry emerges   |

---


## Conclusion

This simulation offers a clear visual demonstration of wave interference principles:

- **Constructive and destructive interference** arise naturally from the superposition of waves.

- The **geometry of the source arrangement** shapes the spatial structure of the resulting pattern.

- These concepts apply broadly to light, sound, and quantum waves, highlighting the fundamental role of interference across all areas of wave physics.

---

