# Problem 3


## **Trajectories of a Freely Released Payload Near Earth**
When a payload is released from a rocket near Earth, its motion is governed by gravitational attraction and its initial velocity. 

The resulting path may be **orbital (elliptical), parabolic, hyperbolic, or reentry** depending on the initial conditions. 



##  **1. Theoretical Principles**

When a payload is released from a moving rocket near Earth, its path is determined by **Newton’s Law of Universal Gravitation** and its **initial velocity vector**.

### **Newton’s Law of Gravitation**
$$
\vec{F} = - \frac{GMm}{r^2} \hat{r}
$$
Where:
- \( G \) = gravitational constant \( (6.674 \times 10^{-11} \, \text{Nm}^2/\text{kg}^2) \)
- \( M \) = mass of Earth \( (5.972 \times 10^{24} \, \text{kg}) \)
- \( m \) = mass of the payload
- \( r \) = distance from Earth’s center to the payload
- \( \hat{r} \) = unit vector in the radial direction

We use **Newton's Second Law** to derive the equation of motion:

$$
\vec{a} = \frac{\vec{F}}{m} = - \frac{GM}{r^2} \hat{r}
$$

This gives us a **second-order differential equation** for motion in 2D space:
$$
\frac{d^2\vec{r}}{dt^2} = - \frac{GM}{|\vec{r}|^3} \vec{r}
$$

### Kepler's Laws
Kepler's Laws describe the motion of planets around the Sun but can be applied to any two-body problem:
1. **Law of Orbits**: Planets move in elliptical orbits with the Sun at one focus.
2. **Law of Areas**: A line joining a planet and the Sun sweeps out equal areas in equal intervals of time.
3. **Law of Periods**: The square of the orbital period of a planet is proportional to the cube of the semi-major axis of its orbit.
---

##  **2. Numerical Simulation of Trajectories**

We'll simulate several types of trajectories based on initial conditions:
- **Elliptical Orbit**: \( v < v_{\text{esc}} \) but tangential
- **Parabolic Escape**: \( v = v_{\text{esc}} \)
- **Hyperbolic Trajectory**: \( v > v_{\text{esc}} \)
- **Ballistic Reentry**: \( v < v_{\text{orb}} \)

###  Definitions
- Escape Velocity:  
$$
v_{\text{esc}} = \sqrt{\frac{2GM}{r}}
$$
- Orbital Velocity:  
$$
v_{\text{orb}} = \sqrt{\frac{GM}{r}}
$$

---

##  3. Implementation 
PGreat! Here's your updated and corrected collapsible code block, ready to paste into your mkDocs `.md` file — now with a properly formatted `<summary>` line and no typos:

```markdown
<details>
<summary>Click to expand Python code for trajectory simulation and plotting</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11       # gravitational constant (m^3/kg/s^2)
M = 5.972e24          # mass of Earth (kg)
R_earth = 6.371e6     # radius of Earth (m)

# Time parameters
dt = 1                # time step in seconds
steps = 30000         # total number of steps (~8 hours)

def simulate_trajectory(r0, v0):
    r = np.array(r0, dtype=float)
    v = np.array(v0, dtype=float)
    
    positions = [r.copy()]
    
    for _ in range(steps):
        r_norm = np.linalg.norm(r)
        a = -G * M * r / r_norm**3
        r += v * dt + 0.5 * a * dt**2
        a_new = -G * M * r / np.linalg.norm(r)**3
        v += 0.5 * (a + a_new) * dt
        positions.append(r.copy())
        
        if np.linalg.norm(r) < R_earth:
            break  # hit the Earth
    
    return np.array(positions)

# Initial position: 300 km above Earth
altitude = 300e3
r0 = np.array([R_earth + altitude, 0.0])

# Case 1: Low velocity (reentry)
v_reentry = np.array([0.0, 3000.0])

# Case 2: Circular orbital velocity
v_orbit = np.array([0.0, np.sqrt(G * M / (R_earth + altitude))])

# Case 3: Escape velocity
v_escape = np.array([0.0, np.sqrt(2 * G * M / (R_earth + altitude))])

# Run simulations
traj_reentry = simulate_trajectory(r0, v_reentry)
traj_orbit = simulate_trajectory(r0, v_orbit)
traj_escape = simulate_trajectory(r0, v_escape)

# Plotting
plt.figure(figsize=(7, 7))
theta = np.linspace(0, 2*np.pi, 500)
earth_x = R_earth * np.cos(theta)
earth_y = R_earth * np.sin(theta)
plt.plot(earth_x, earth_y, color='black', label='Earth')

plt.plot(traj_reentry[:,0], traj_reentry[:,1], label='Reentry', linestyle='--')
plt.plot(traj_orbit[:,0], traj_orbit[:,1], label='Orbital', linestyle='-')
plt.plot(traj_escape[:,0], traj_escape[:,1], label='Escape', linestyle=':')

plt.axis('equal')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Comparison of Payload Trajectories at Different Velocities')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
```

</details>
```



###  **Simulation Result: Payload Trajectories Near Earth**

![alt text](<Comparison Of Payload Trajectories At Different Velocities.png>)

Here's what you're seeing in the plot:


| Label                 | Behavior                                      | Type of Orbit       |
|----------------------|-----------------------------------------------|---------------------|
| **Suborbital (4 km/s)**     | Falls back to Earth                         | **Reentry** / crash |
| **Orbital (7.7 km/s)**      | Maintains circular/elliptical orbit         | **Bound Orbit**     |
| **Escape (11.2 km/s)**      | Just escapes Earth’s gravity                | **Parabolic Escape**|
| **Super Escape (13 km/s)**  | Strong escape path, flies away rapidly      | **Hyperbolic**      |

---

## 🛰 What You Can Learn:
- **Energy = 0** at escape velocity ⇒ parabolic path  
- **Energy < 0** ⇒ elliptical or suborbital trajectory  
- **Energy > 0** ⇒ hyperbolic trajectory (leaves forever!)

This is the foundation for **orbital insertion**, **interplanetary transfers**, and **return trajectories**.


## 📌 Key Physics Insights

| Trajectory Type      | Condition                        | Application                           |
|----------------------|----------------------------------|----------------------------------------|
| Suborbital           | \( v < v_{\text{orb}} \)         | Missile arcs, space tourism            |
| Orbital              | \( v = v_{\text{orb}} \)         | Satellites, space stations             |
| Parabolic Escape     | \( v = v_{\text{esc}} \)         | Theoretical edge of gravity            |
| Hyperbolic Escape    | \( v > v_{\text{esc}} \)         | Interplanetary missions, Voyager       |

