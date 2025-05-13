# Problem 2
# Estimating \( \pi \) Using Monte Carlo Methods
## Part 1: Circle-Based Simulation

## Motivation

Monte Carlo methods use random sampling to solve problems that might be difficult to approach analytically. One elegant use of this technique is estimating the value of \( \pi \) through geometric probability.

By randomly generating points inside a square that encloses a circle, the ratio of points landing inside the circle reveals an approximation of \( \pi \). This approach not only demonstrates the power of randomness but also connects probability, geometry, and numerical computation in a visually intuitive way.

### Theoretical Foundation

Consider a square of side length 2, centered at the origin, enclosing a unit circle (radius = 1). The area of the square is:

$$
A_{\text{square}} = (2)^2 = 4
$$

The area of the circle is:

$$
A_{\text{circle}} = \pi \cdot 1^2 = \pi
$$

If points are randomly and uniformly distributed within the square, the probability of a point landing inside the circle is the ratio of their areas:

$$
P(\text{inside circle}) = \frac{\pi}{4}
$$

Rearranging gives an estimate for \( \pi \):

$$
\pi \approx 4 \cdot \left( \frac{\text{Number of points inside the circle}}{\text{Total number of points}} \right)
$$

This result forms the basis of the circle-based Monte Carlo method for estimating \( \pi \).

### Simulation and Visualization

![alt text](<Monte Carlo Estimate of π (n = {n_points})nEstimate = {pi_estimate.5f}.png>)

> **Fig. :** Points are uniformly distributed across a square. Points inside the unit circle are shown in blue, and those outside are shown in red. The ratio of inside-to-total points, multiplied by 4, provides a Monte Carlo estimate of \( \pi \).

<details>
<summary>Click to Expand Simulation and Visualization Code</summary>

<pre><code>
```python
import numpy as np
import matplotlib.pyplot as plt

n_points = 5000

# Generate random points in the square [-1, 1] x [-1, 1]
x = np.random.uniform(-1, 1, n_points)
y = np.random.uniform(-1, 1, n_points)

# Check which points fall inside the unit circle
inside_circle = x**2 + y**2 <= 1

# Estimate π
pi_estimate = 4 * np.sum(inside_circle) / n_points

# Plot the simulation
plt.figure(figsize=(6, 6))
plt.scatter(x[~inside_circle], y[~inside_circle], s=1, color='red', label='Outside Circle')
plt.scatter(x[inside_circle], y[inside_circle], s=1, color='blue', label='Inside Circle')
plt.gca().set_aspect('equal')
plt.title(f"Monte Carlo Estimate of π (n = {n_points})\nEstimate = {pi_estimate:.5f}")
plt.xlabel("x")
plt.ylabel("y")
plt.legend(loc='lower left')
plt.grid(True)
plt.savefig("pi_estimate_circle_method.png")
plt.close()
</code></pre>

</details>

---
### Convergence Analysis

![alt text](<Convergence of Monte Carlo π Estimate.png>)

> **Fig. :** Estimated values of \( \pi \) are plotted against increasing sample sizes. The curve approaches the true value, demonstrating how accuracy improves with more samples in the Monte Carlo method.

## Part 2: Buffon’s Needle Simulation

### Theoretical Foundation

Buffon’s Needle is a classical probability problem that provides a method for estimating \( \pi \) based on random needle drops on a lined surface.

Imagine a floor with parallel lines spaced a distance \( d \) apart. A needle of length \( l \) is randomly dropped onto this surface. The probability that the needle crosses a line is:

$$
P = \frac{2l}{\pi d}
$$

Rearranging this formula provides an estimate of \( \pi \):

$$
\pi \approx \frac{2l \cdot N}{d \cdot C}
$$

Where:
- \( N \) is the number of needle drops,

- \( C \) is the number of times the needle crosses a line,

- \( l \) is the length of the needle (must be \( \leq d \)),

- \( d \) is the distance between the lines.

This geometric approach uses the probability of intersection to infer the value of \( \pi \).

### Simulation and Visualization

![alt text](<Buffon’s Needle Simulation (n = {n_needles})nEstimate = {pi_buffon.5f}.png>)

> **Fig. :** Randomly dropped needles (gray and blue) are shown over a lined background. Blue needles indicate those that intersect with a line. The proportion of intersections is used to estimate the value of \( \pi \) using Buffon’s formula.

<details>
<summary>Click to Expand Simulation and Visualization Code</summary>

<pre><code>
```python
import numpy as np
import matplotlib.pyplot as plt

needle_length = 1.0
line_spacing = 2.0
n_needles = 1000

# Generate random needle positions and orientations
x_center = np.random.uniform(0, line_spacing / 2, n_needles)
theta = np.random.uniform(0, np.pi, n_needles)

# Determine which needles cross a line
crossings = x_center <= (needle_length / 2) * np.sin(theta)

# Estimate π
pi_buffon = (2 * needle_length * n_needles) / (line_spacing * np.sum(crossings))

# Compute endpoints for needle drawing
x1 = x_center - (needle_length / 2) * np.cos(theta)
x2 = x_center + (needle_length / 2) * np.cos(theta)
y1 = -(needle_length / 2) * np.sin(theta)
y2 = (needle_length / 2) * np.sin(theta)

# Plot the simulation
plt.figure(figsize=(8, 6))
for i in range(n_needles):
    color = 'blue' if crossings[i] else 'gray'
    plt.plot([x1[i], x2[i]], [y1[i], y2[i]], color=color, linewidth=0.8)

# Draw the parallel lines
for x in np.arange(0, line_spacing * 3, line_spacing):
    plt.axvline(x=x, color='black', linestyle='--', linewidth=0.6)

plt.xlim(0, line_spacing * 3)
plt.ylim(-1.2, 1.2)
plt.title(f"Buffon’s Needle Simulation (n = {n_needles})\nEstimate = {pi_buffon:.5f}")
plt.xlabel("x")
plt.ylabel("y")
plt.grid(True)
plt.savefig("buffon_needle_simulation.png")
plt.close()

</code></pre>

</details>

---

### Convergence Analysis

![alt text](<Convergence of π Estimate – Buffon’s Needle Method.png>)

> **Fig. :** The plot shows how the Buffon’s Needle estimate of \( \pi \) changes with increasing numbers of needle drops. While convergence is visible, the estimates fluctuate more than in the circle method, reflecting higher variability in this approach.

<details>
<summary>Click to Expand Convergence Analysis Code</summary>

<pre><code>
```python
needle_trials = [100, 500, 1000, 5000, 10000, 50000]
buffon_estimates = []

needle_length = 1.0
line_spacing = 2.0

for n in needle_trials:
    x_center = np.random.uniform(0, line_spacing / 2, n)
    theta = np.random.uniform(0, np.pi, n)
    crossings = x_center <= (needle_length / 2) * np.sin(theta)
    estimate = (2 * needle_length * n) / (line_spacing * np.sum(crossings))
    buffon_estimates.append(estimate)

plt.figure(figsize=(8, 5))
plt.plot(needle_trials, buffon_estimates, marker='o', label='Estimated π')
plt.axhline(np.pi, color='gray', linestyle='--', label='Actual π')
plt.title("Convergence of π Estimate – Buffon’s Needle Method")
plt.xlabel("Number of Needle Drops")
plt.ylabel("Estimated π")
plt.legend()
plt.grid(True)
plt.savefig("pi_convergence_buffon_method.png")
plt.close()

</code></pre>

</details>

---
## Conclusion

Two Monte Carlo methods were used to estimate the value of \( \pi \): the circle-based method and Buffon’s Needle simulation.

- **The circle-based method** demonstrated fast and stable convergence. The estimate improved smoothly as more points were added, with relatively low variance across runs.

- **Buffon’s Needle** produced accurate estimates but with higher variability, especially for smaller sample sizes. Its geometric setup is more complex and sensitive to random angle distributions.

Both approaches confirm the effectiveness of randomness in numerical estimation. While the circle method is more efficient for this task, Buffon’s Needle remains a valuable demonstration of geometric probability and the historical roots of Monte Carlo simulation.
