# Problem 1

Step 1: Theoretical Foundation
Projectile motion follows Newton’s equations of motion in two dimensions. The horizontal and vertical components of motion are treated separately:

Equations of Motion
Horizontal Motion:

𝑥
=
𝑣
0
cos
⁡
(
𝜃
)
𝑡
x=v 
0
​
 cos(θ)t
The velocity remains constant in the absence of air resistance.
Vertical Motion:

𝑦
=
𝑣
0
sin
⁡
(
𝜃
)
𝑡
−
1
2
𝑔
𝑡
2
y=v 
0
​
 sin(θ)t− 
2
1
​
 gt 
2
 
The vertical motion is influenced by gravity 
𝑔
g, causing acceleration downward.
Time of Flight:

The projectile reaches the ground when 
𝑦
=
0
y=0, solving for 
𝑡
t:
𝑡
𝑓
=
2
𝑣
0
sin
⁡
(
𝜃
)
𝑔
t 
f
​
 = 
g
2v 
0
​
 sin(θ)
​
 
Range Equation:

Using the time of flight in the horizontal equation:
𝑅
=
𝑣
0
cos
⁡
(
𝜃
)
⋅
2
𝑣
0
sin
⁡
(
𝜃
)
𝑔
R=v 
0
​
 cos(θ)⋅ 
g
2v 
0
​
 sin(θ)
​
 
𝑅
=
𝑣
0
2
sin
⁡
(
2
𝜃
)
𝑔
R= 
g
v 
0
2
​
 sin(2θ)
​
 
This equation shows that range is maximized at 
𝜃
=
45
∘
θ=45 
∘
 .

Step 2: Analysis of the Range
The range depends on:

Launch speed 
𝑣
0
v 
0
​
 
Gravity 
𝑔
g (stronger gravity reduces range)
Launch angle 
𝜃
θ
Predictions:

The function 
𝑅
(
𝜃
)
=
𝑣
0
2
sin
⁡
(
2
𝜃
)
𝑔
R(θ)= 
g
v 
0
2
​
 sin(2θ)
​
  has a symmetric shape.
Maximum range is at 
𝜃
=
45
∘
θ=45 
∘
 .
Complementary angles (e.g., 30° and 60°) yield the same range.
Step 3: Practical Applications
Sports: Soccer, basketball, golf, etc.
Engineering: Missile trajectories, cannonball physics.
Astrophysics: Spacecraft landings on different planets (varying 
𝑔
g).
Military Science: Optimal angles for artillery fire.
Step 4: Implementation in Python
Now, let’s implement this in VS Code using Python. The script will:

Compute the range for different angles.
Plot the range-angle relationship.
Show the effect of varying 
𝑣
0
v 
0
​
  and 
𝑔
g.
Python Code (Projectile Range Simulation)
Save this as projectile_range.py in your VS Code workspace.

python
Copy
Edit
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
Step 5: Discussion on Model Limitations
While the model is idealized, real-world scenarios include:

Air Resistance – Causes energy loss and reduces range.
Uneven Terrain – Changes landing conditions.
Wind Effects – Alters trajectory.
Spin Effects – In sports (e.g., Magnus effect in soccer).
How to Extend the Model
Add air resistance: Modify the equations to include a drag force.
Vary gravity: Simulate motion on the Moon or Mars.
Simulate bounces: Implement ground interaction for multiple landings.
Deliverables
Python Script (projectile_range.py) – For visualization.
Markdown Report – Include the theoretical background and code explanations.
Plots – Show how range depends on angle and velocity.