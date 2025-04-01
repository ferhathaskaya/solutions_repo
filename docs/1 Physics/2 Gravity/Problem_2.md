# Problem 2
## **Escape Velocities and Cosmic Velocities**

### 1. Definitions & Physical Meaning

#### a. **First Cosmic Velocity (Orbital Velocity):**
  - Minimum speed to achieve a stable circular orbit around a planet.
#### b. **Second Cosmic Velocity (Escape Velocity):**
  - Minimum speed to escape a planet’s gravitational field without further propulsion.
#### c. **Third Cosmic Velocity (Interstellar Escape):**
  - Minimum speed to leave the gravitational influence of the Sun (or star), starting from a planet.

#### **These are fundamental in planning:**
- Satellite launches
- Interplanetary missions (e.g., to Mars, Jupiter)
- Dreams of interstellar travel

### 2.  Mathematical Derivations

#### a. First Cosmic Velocity $v_1$ (Orbital Velocity)
Minimum speed to stay in circular orbit close to surface:

$$
v_1 = \sqrt{\frac{GM}{r}}
$$

- **\(G\)**: Gravitational constant
- **\(M\)**: Mass of the planet
- **\(r\)**: Distance from the planet’s center

#### b. Second Cosmic Velocity $v_2$ (Escape Velocity)
Minimum speed to break free from gravity:
$$
v_2 = \sqrt{2} \cdot v_1 = \sqrt{\frac{2GM}{r}}
$$

#### c. Third Cosmic Velocity  $v_3$ (Solar System Escape)
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


###  **Earth**

**Given**:  
\( M = 5.972 \times 10^{24} \, \text{kg} \)  
\( R = 6.371 \times 10^6 \, \text{m} \)  
\( r_{\text{orbit}} = 1 \, \text{AU} = 1.496 \times 10^{11} \, \text{m} \)

$$
v_1 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 5.972 \times 10^{24}}{6.371 \times 10^6}} \approx \boxed{7,910 \, \text{m/s}}
$$

$$
v_2 = \sqrt{2} \cdot 7,910 \approx \boxed{11,186 \, \text{m/s}}
$$

$$
v_3 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 1.989 \times 10^{30}}{1.496 \times 10^{11}}} \approx \boxed{29,789 \, \text{m/s}}
$$

---

###  **Mars**

**Given**:  
\( M = 6.39 \times 10^{23} \, \text{kg} \)  
\( R = 3.3895 \times 10^6 \, \text{m} \)  
\( r_{\text{orbit}} = 1.524 \, \text{AU} = 2.279 \times 10^{11} \, \text{m} \)

$$
v_1 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 6.39 \times 10^{23}}{3.3895 \times 10^6}} \approx \boxed{3,547 \, \text{m/s}}
$$

$$
v_2 = \sqrt{2} \cdot 3,547 \approx \boxed{5,016 \, \text{m/s}}
$$

$$
v_3 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 1.989 \times 10^{30}}{2.279 \times 10^{11}}} \approx \boxed{24,130 \, \text{m/s}}
$$

---

###  **Jupiter**

**Given**:  
\( M = 1.898 \times 10^{27} \, \text{kg} \)  
\( R = 6.9911 \times 10^7 \, \text{m} \)  
\( r_{\text{orbit}} = 5.204 \, \text{AU} = 7.78 \times 10^{11} \, \text{m} \)

$$
v_1 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 1.898 \times 10^{27}}{6.9911 \times 10^7}} \approx \boxed{42,568 \, \text{m/s}}
$$

$$
v_2 = \sqrt{2} \cdot 42,568 \approx \boxed{60,200 \, \text{m/s}}
$$

$$
v_3 = \sqrt{\frac{6.674 \times 10^{-11} \cdot 1.989 \times 10^{30}}{7.78 \times 10^{11}}} \approx \boxed{13,058 \, \text{m/s}}
$$

---


**Cosmic Velocities Table**

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


