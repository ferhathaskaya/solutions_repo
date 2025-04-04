# Problem 2
##  **Escape Velocities and Cosmic Velocities**

Understanding how fast an object needs to go to stay in orbit, escape a planet, or leave the entire Solar System is fundamental in space exploration and mission design.

---

###  1. Definitions & Physical Meaning

####  **a. First Cosmic Velocity (Orbital Velocity)**  
> *The minimum speed required to maintain a stable circular orbit close to the planet's surface.*

$$
v_1 = \sqrt{\frac{GM}{R}}
$$

- Used to place satellites into low Earth orbit (LEO).
- Example: ISS orbits at ~7.66 km/s around Earth.

---

####  **b. Second Cosmic Velocity (Escape Velocity)**  
> *The minimum speed needed to escape a planet’s gravity without further propulsion.*

$$
v_2 = \sqrt{2} \cdot v_1 = \sqrt{\frac{2GM}{R}}
$$

- Used in missions to the Moon, Mars, etc.
- Example: Earth escape velocity is ~11.2 km/s.

---

####  **c. Third Cosmic Velocity (Solar Escape Velocity)**  
> *The speed required to escape the gravitational influence of the Sun starting from a planet's orbit.*

$$
v_3 = \sqrt{\frac{GM_\odot}{r_{\text{orbit}}}}
$$

- Needed for interstellar probes like Voyager 1.
- Lower at planets farther from the Sun (e.g., Jupiter).

---

### 2.  Mathematical Derivations

#### a. First Cosmic Velocity $v_1$
Minimum speed to stay in circular orbit close to surface:

$$
v_1 = \sqrt{\frac{GM}{r}}
$$

- **\(G\)**: Gravitational constant
- **\(M\)**: Mass of the planet
- **\(r\)**: Distance from the planet’s center

#### b. Second Cosmic Velocity $v_2$
Minimum speed to break free from gravity:
$$
v_2 = \sqrt{2} \cdot v_1 = \sqrt{\frac{2GM}{r}}
$$

#### c. Third Cosmic Velocity  $v_3$
Speed to escape Sun’s gravity from Earth's orbit:
$$
v_3 = \sqrt{v_{esc,Sun}^2 + v_{planet-orbit}^2}
$$
#### Where:
- $$v_{esc,Sun} = \sqrt{\frac{2GM_{Sun}}{r_{orbit}}}$$
- $$v_{planet-orbit} = \sqrt{\frac{GM_{Sun}}{r_{orbit}}}$$

---

### **3. Calculations for Earth, Mars, and Jupiter**

Here are the **Cosmic Velocities** for **Earth**, **Mars**, and **Jupiter**.



| Planet   | 1st Cosmic (Orbital) | 2nd Cosmic (Escape) | 3rd Cosmic (Solar Escape) |
|----------|----------------------|----------------------|----------------------------|
| **Earth**   | ~7.91 km/s               | ~11.19 km/s               | ~29.79 km/s                      |
| **Mars**    | ~3.55 km/s               | ~5.02 km/s                | ~24.13 km/s                   |
| **Jupiter** | ~42.57 km/s              | ~60.20 km/s               |  ~13.05 km/s                   |



### 4. **Graphical Representations of Cosmic Velocities**

![alt text](<Comparison Of 1st, 2nd, And 3rd Cosmic Velocities.png>)

- **1st Cosmic Velocity (Orbit)**: Needed to stay in low orbit.

- **2nd Cosmic Velocity (Escape)**: Needed to break free from gravity.

- **3rd Cosmic Velocity (Solar Escape)**: Only shown for Earth in this case — needed to escape the Solar System.

---

###  **5. Relevance in Space Exploration and Applications**


| Velocity | Application |
|----------|-------------|
| **1st Cosmic** | Required for artificial satellites, space stations. Example: GPS, Starlink, ISS.|

![alt text](<1st Cosmic Velocity From Earth.png>)

- (**Orbit**: The spacecraft keeps circling around the planet.)

| Velocity | Application |
|----------|-------------|
| **2nd Cosmic** | Required for planetary missions, launching space probes beyond Earth.  Example: Moon Missions, Apollo missions, Mars rovers. |

![alt text](<2nd Cosmic Velocity From Earth.png>)

- (**Escape**: The path shows a parabolic escape, the spacecraft is going fast enough to break free from Earth’s gravity.)

| Velocity | Application |
|----------|-------------|
| **3rd Cosmic** | Required for interstellar missions. Example: Voyager 1 & 2 missions. |

![alt text](<3rd Cosmic Velocity From Earth.png>)

- (**Solar System Escape**: A fast, hyperbolic escape trajectory; this craft will leave Earth and the entire Solar System.)


### **Significance in Mission Planning**:
- Determines **fuel requirements** and **launch energy**.
- Affects **rocket design**, **payload mass**, and **mission duration**.
- Vital for **interplanetary and interstellar travel**.


