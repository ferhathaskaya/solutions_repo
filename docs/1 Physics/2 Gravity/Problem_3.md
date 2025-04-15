# Problem 3
# Trajectories of a Freely Released Payload Near Earth

When a payload is released from a rocket near Earth, its motion is influenced by two key factors:

- **Gravitational attraction** from Earth
- Its **initial velocity** and direction at the moment of release

Depending on these conditions, the payload can follow different paths:

- **Orbital (Elliptical)**: bound motion around Earth  
- **Parabolic**: just enough speed to escape Earth’s gravity  
- **Hyperbolic**: exceeds escape speed, escaping rapidly  
- **Reentry**: insufficient speed, falls back to Earth

---

## 1. Theoretical Principles

The motion of the payload is governed by:

- **Newton’s Law of Universal Gravitation**
- The **initial velocity vector** of the payload at release

These determine whether the object enters orbit, escapes, or returns to Earth.


### Newton’s Law of Gravitation

The gravitational force between two masses is given by:

$$
\vec{F} = - \frac{GMm}{r^2} \, \hat{r}
$$

**Where:**

- \( G \): Gravitational constant  
  \( = 6.674 \times 10^{-11} \, \text{N·m}^2/\text{kg}^2 \)

- \( M \): Mass of the Earth  
  \( = 5.972 \times 10^{24} \, \text{kg} \)

- \( m \): Mass of the payload

- \( r \): Distance from Earth's center to the payload

- \( \hat{r} \): Unit vector pointing radially outward from Earth to the payload


We use **Newton's Second Law** to derive the equation of motion:

$$
\vec{a} = \frac{\vec{F}}{m} = - \frac{GM}{r^2} \hat{r}
$$

This gives us a **second-order differential equation** for motion in 2D space:
$$
\frac{d^2\vec{r}}{dt^2} = - \frac{GM}{|\vec{r}|^3} \vec{r}
$$

###  Kepler’s Laws

Kepler’s Laws describe the motion of planets around the Sun and can be applied to any two-body orbital system:

1. **Law of Orbits**  
   > Planets move in **elliptical orbits** with the Sun at one focus.

2. **Law of Areas**  
   > A line joining a planet and the Sun sweeps out **equal areas** in **equal intervals of time**.

3. **Law of Periods**  
   > The square of a planet’s orbital period is **proportional to the cube** of the **semi-major axis** of its orbit.

---

##  2. Numerical Simulation of Trajectories

We simulate several types of trajectories based on the **initial speed and direction** of the payload:

- **Elliptical Orbit**  
  \( v < v_{\text{esc}} \) (sub-escape speed, tangential to surface)

- **Parabolic Escape**  
  \( v = v_{\text{esc}} \) (just enough speed to escape Earth’s gravity)

- **Hyperbolic Trajectory**  
  \( v > v_{\text{esc}} \) (excess speed; the object escapes on an open path)

- **Ballistic Reentry**  
  \( v < v_{\text{orb}} \) (too slow to maintain orbit, falls back to Earth)


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
<details>
<summary>Click to expand full simulation code for velocity, altitude, and angle comparisons</summary>

<pre><code>
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

</code></pre>

</details>



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

## Key Physics Insights:
- **Energy = 0** at escape velocity ⇒ parabolic path  
- **Energy < 0** ⇒ elliptical or suborbital trajectory  
- **Energy > 0** ⇒ hyperbolic trajectory

##  4. Payload Trajectory Comparisons

## Python Implementation for Trajectory Simulation and Visualization
This implementation is used for all comparisons shown in the plots. The velocity, altitude, and angle parameters are varied to illustrate their effects on orbital behavior.

<details>
<summary>Click to expand full simulation code for velocity, altitude, and angle comparisons</summary>

<pre><code>
    import numpy as np
    import matplotlib.pyplot as plt

    # Constants
    G = 6.67430e-11       # gravitational constant (m^3/kg/s^2)
    M = 5.972e24          # mass of Earth (kg)
    R_earth = 6.371e6     # radius of Earth (m)
    dt = 1                # time step (s)
    steps = 30000         # number of integration steps

    # Trajectory simulation function
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
                break  # impact with Earth
        return np.array(positions)

    # Earth outline for plotting
    theta = np.linspace(0, 2 * np.pi, 500)
    earth_x = R_earth * np.cos(theta)
    earth_y = R_earth * np.sin(theta)

    # --------------------
    # Velocity Comparison
    # --------------------
    velocity_cases = {
        "3.0 km/s": 3000,
        "4.0 km/s": 4000,
        "6.0 km/s": 6000,
        "7.7 km/s": 7700,
        "9.0 km/s": 9000,
        "11.2 km/s": 11200,
        "13.0 km/s": 13000
    }
    r0_velocity = np.array([R_earth + 300e3, 0.0])
    velocity_trajectories = {}
    for label, speed in velocity_cases.items():
        v0 = [0, speed]
        velocity_trajectories[label] = simulate_trajectory(r0_velocity, v0)

    plt.figure(figsize=(8, 8))
    plt.plot(earth_x / 1e3, earth_y / 1e3, 'k', label='Earth')
    for label, traj in velocity_trajectories.items():
        plt.plot(traj[:, 0] / 1e3, traj[:, 1] / 1e3, label=label)
    plt.title("Velocity-Based Trajectories")
    plt.xlabel("x (km)")
    plt.ylabel("y (km)")
    plt.axis('equal')
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()

    # --------------------
    # Altitude Comparison
    # --------------------
    altitudes_km = [100, 300, 500, 1000]
    altitude_trajectories = {}
    speed = 7700
    for alt in altitudes_km:
        r0 = np.array([R_earth + alt * 1e3, 0.0])
        v0 = [0, speed]
        altitude_trajectories[f"{alt} km"] = simulate_trajectory(r0, v0)

    plt.figure(figsize=(8, 8))
    plt.plot(earth_x / 1e3, earth_y / 1e3, 'k', label='Earth')
    for label, traj in altitude_trajectories.items():
        plt.plot(traj[:, 0] / 1e3, traj[:, 1] / 1e3, label=label)
    plt.title("Altitude-Based Trajectories")
    plt.xlabel("x (km)")
    plt.ylabel("y (km)")
    plt.axis('equal')
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()

    # --------------------
    # Angle Comparison
    # --------------------
    angles_deg = [0, 30, 45, 60, 90, 120]
    angle_trajectories = {}
    speed = 7700
    r0_angle = np.array([R_earth + 300e3, 0.0])
    for angle in angles_deg:
        angle_rad = np.radians(angle)
        vx = speed * np.cos(angle_rad)
        vy = speed * np.sin(angle_rad)
        v0 = [vx, vy]
        angle_trajectories[f"{angle}°"] = simulate_trajectory(r0_angle, v0)

    plt.figure(figsize=(8, 8))
    plt.plot(earth_x / 1e3, earth_y / 1e3, 'k', label='Earth')
    for label, traj in angle_trajectories.items():
        plt.plot(traj[:, 0] / 1e3, traj[:, 1] / 1e3, label=label)
    plt.title("Angle-Based Trajectories")
    plt.xlabel("x (km)")
    plt.ylabel("y (km)")
    plt.axis('equal')
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()
</code></pre>

</details>



**Velocity Comparison**

![alt text](<Velocity-Based Trajectories (300 Km Altitude, 90° Angle) (2).png>)

## Velocity Comparisons

Here, we fix the **altitude to 300 km** and **launch angle to 90°**, and vary the **initial speed**.

| **Speed (km/s)** | **Resulting Orbit Type**        |
| ---------------- | ------------------------------- |
| 3.0              | Immediate reentry               |
| 4.0              | Suborbital, falls back to Earth |
| 6.0              | Partial orbit, returns          |
| 7.7              | Circular or elliptical orbit    |
| 9.0              | Elongated elliptical orbit      |
| 11.2             | Parabolic escape                |
| 13.0             | Hyperbolic escape               |

This table illustrates how increasing speed increases orbital range and determines escape conditions.


Showing how different initial speeds at 300 km altitude (launched vertically) affect the trajectory:

- Low speeds (3–4 km/s): reentry
- Medium speeds (6–9 km/s): elliptical orbits
- High speeds (11.2+ km/s): escape trajectories



**Altitude Comparison**

![alt text](<Altitude-Based Trajectories (Speed = 7.7 KmS, 90° Angle).png>)


Here, we fix the **speed to 7.7 km/s** (near orbital speed) and vary the **altitude**. Launch angle is kept at 90°.

| **Altitude (km)** | **Resulting Orbit**                       |
| ----------------- | ----------------------------------------- |
| 100               | Smaller elliptical orbit, lower stability |
| 300               | Stable low Earth orbit                    |
| 500               | Broader elliptical orbit                  |
| 1000              | Long-period elliptical orbit              |

- As altitude increases (100 km → 1000 km), the orbits become wider and more stable.
- All trajectories start at 90° angle with 7.7 km/s speed — showing how **altitude alone** influences the orbital shape and duration.

**Angle Comparison**

![alt text](<Angle-Based Trajectories (300 Km Altitude, Speed = 7.7 KmS).png>)


Here, we fix the **altitude to 300 km** and **speed to 7.7 km/s**, and vary the **launch angle** from horizontal (0°) to vertical (90°) and beyond.

| **Angle (°)** | **Resulting Orbit Type**            |
| ------------- | ----------------------------------- |
| 0             | Skimming trajectory, reentry likely |
| 30            | Low arc, short elliptical path      |
| 45            | Optimal for range, stable ellipse   |
| 60            | Higher arc, longer orbit            |
| 90            | High-altitude ellipse or circular   |
| 120           | Retrograde-like arc, steep reentry  |

This comparison shows how the launch direction affects orbital characteristics and stability even at the same speed and altitude.


This comparison shows how the launch direction affects orbital characteristics and stability even at the same speed and altitude

- All payloads are released from 300 km altitude at **7.7 km/s**, but with varying angles.
- You can see:
  - **0° (horizontal)** leads to near-surface skimming and reentry.
  - **45°–90°** create stable orbits.
  - **120°** launches the payload backward into a steep downward arc.


