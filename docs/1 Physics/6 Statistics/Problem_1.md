# Problem 1
# Exploring the Central Limit Theorem through Simulations

## Motivation

The Central Limit Theorem (CLT) shows that sample means tend to follow a normal distribution as sample size increases, regardless of the original data's shape.

This principle is key to many statistical methods. Simulations help us see how and when this convergence occurs.

## Theoretical Background

Let \( X_1, X_2, \dots, X_n \) be independent and identically distributed (i.i.d.) random variables with mean \( \mu \) and finite variance \( \sigma^2 \). The CLT states that the standardized sample mean:

$$
Z = \frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}}
$$

converges in distribution to the standard normal distribution as \( n \to \infty \):

$$
Z \xrightarrow{d} \mathcal{N}(0, 1)
$$

This holds even when the original population distribution is not normal, provided the variance is finite.

## Simulation Setup

This analysis examines the Central Limit Theorem using three types of population distributions:

- **Uniform distribution**: values spread evenly over an interval.

- **Exponential distribution**: a skewed, right-tailed distribution.

- **Binomial distribution**: discrete outcomes based on repeated trials.

For each distribution:

- A large synthetic population is generated (e.g., 100,000 values).

- Samples of sizes **5, 10, 30, and 50** are drawn repeatedly (e.g., 1000 times).

- The mean of each sample is recorded.

- Histograms of the resulting sample means are used to observe convergence toward a normal distribution as sample size increases.

### Uniform Distribution – Sampling Distribution of the Sample Mean

Symmetric and bounded, the sampling distribution of the mean was approximately normal even for small sample sizes.


![alt text](<Sampling Distribution of Sample Means – Uniform Population.png>)

> **Fig. :** Sampling distributions of the sample mean for a uniform population. As sample size increases from 5 to 50, the distribution of sample means becomes increasingly bell-shaped, illustrating the Central Limit Theorem.

<details>
<summary>Click to Expand Uniform Distribution Code</summary>

<pre><code>
```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

np.random.seed(42)
population_size = 100_000
sample_sizes = [5, 10, 30, 50]
n_samples = 1000

# Generate uniform population
uniform_population = np.random.uniform(low=0, high=1, size=population_size)

# Plot sampling distributions
fig, axes = plt.subplots(2, 2, figsize=(12, 8))
axes = axes.flatten()

for i, n in enumerate(sample_sizes):
    sample_means = [
        np.mean(np.random.choice(uniform_population, size=n, replace=False))
        for _ in range(n_samples)
    ]
    sns.histplot(sample_means, bins=30, kde=True, ax=axes[i], color='steelblue')
    axes[i].set_title(f"Sample Size = {n}")
    axes[i].set_xlabel("Sample Mean")
    axes[i].set_ylabel("Frequency")

plt.suptitle("Sampling Distribution of Sample Means – Uniform Population", fontsize=14)
plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.savefig("clt_uniform_distribution.png")
plt.close()


</code></pre>

</details>

---
### Exponential Distribution – Sampling Distribution of the Sample Mean

Highly skewed, the sample means began skewed for small sizes but became nearly normal as sample size increased, clearly demonstrating the CLT in action.

![alt text](<Sampling Distribution of Sample Means – Exponential Population.png>)

> **Fig. :** Sampling distributions of the sample mean for an exponential population. Despite the population’s skewed shape, the sample means become approximately normal as sample size increases, confirming the Central Limit Theorem.

<details>
<summary>Click to Expand Exponential Distribution Code</summary>

<pre><code>
```python
# Generate exponential population
exp_population = np.random.exponential(scale=1.0, size=100_000)

sample_sizes = [5, 10, 30, 50]
n_samples = 1000

fig, axes = plt.subplots(2, 2, figsize=(12, 8))
axes = axes.flatten()

for i, n in enumerate(sample_sizes):
    sample_means = [
        np.mean(np.random.choice(exp_population, size=n, replace=False))
        for _ in range(n_samples)
    ]
    sns.histplot(sample_means, bins=30, kde=True, ax=axes[i], color='darkorange')
    axes[i].set_title(f"Sample Size = {n}")
    axes[i].set_xlabel("Sample Mean")
    axes[i].set_ylabel("Frequency")

plt.suptitle("Sampling Distribution of Sample Means – Exponential Population", fontsize=14)
plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.savefig("clt_exponential_distribution.png")
plt.close()
</code></pre>

</details>

---
### Binomial Distribution – Sampling Distribution of the Sample Mean

Discrete but symmetric, the sampling distribution of the mean quickly approached normality, especially for \( n \geq 30 \).

![alt text](<Sampling Distribution of Sample Means – Binomial Population.png>)

> **Fig. :** Sampling distributions of the sample mean for a binomial population. As sample size increases, the distribution of sample means becomes more symmetric and approaches a normal shape, consistent with the Central Limit Theorem.

<details>
<summary>Click to Expand Binomial Distribution Code</summary>

<pre><code>
```python
# Generate binomial population (n=10 trials, p=0.5)
binom_population = np.random.binomial(n=10, p=0.5, size=100_000)

sample_sizes = [5, 10, 30, 50]
n_samples = 1000

fig, axes = plt.subplots(2, 2, figsize=(12, 8))
axes = axes.flatten()

for i, n in enumerate(sample_sizes):
    sample_means = [
        np.mean(np.random.choice(binom_population, size=n, replace=False))
        for _ in range(n_samples)
    ]
    sns.histplot(sample_means, bins=30, kde=True, ax=axes[i], color='seagreen')
    axes[i].set_title(f"Sample Size = {n}")
    axes[i].set_xlabel("Sample Mean")
    axes[i].set_ylabel("Frequency")

plt.suptitle("Sampling Distribution of Sample Means – Binomial Population", fontsize=14)
plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.savefig("clt_binomial_distribution.png")
plt.close()

</code></pre>

</details>

---

## Parameter Exploration

The simulations reveal several key observations about how the Central Limit Theorem behaves under different conditions:

- **Effect of Sample Size:**  
  As the sample size increases (from 5 to 50), the sampling distribution of the mean becomes increasingly bell-shaped. Even highly skewed distributions, like the exponential, show clear convergence to a normal distribution at larger sample sizes.

- **Effect of Distribution Shape:**  
  The original population shape strongly influences the sampling distribution at small sample sizes. For example, sample means from a skewed exponential population remain skewed when \( n = 5 \), but become nearly normal by \( n = 30 \) or \( 50 \).

- **Effect of Variance:**  
  Populations with higher variance lead to wider sampling distributions. However, as the sample size increases, the spread decreases proportionally to \( 1/\sqrt{n} \), which matches the theoretical result:
  
$$
  \text{Standard deviation of sample mean} = \frac{\sigma}{\sqrt{n}}
$$

These patterns align with the Central Limit Theorem’s prediction that the distribution of sample means stabilizes and becomes normal as \( n \) increases, regardless of the original distribution’s shape.

## Applications of the Central Limit Theorem

The Central Limit Theorem plays a critical role in many statistical methods and real-world applications:

- **Estimation and Confidence Intervals:**  
  The CLT allows the use of normal-based confidence intervals for sample means, even when the population is not normally distributed.

- **Hypothesis Testing:**  
  Many test statistics rely on the assumption that sampling distributions are approximately normal under the null hypothesis.

- **Quality Control:**  
  In manufacturing, averages of measurements (e.g., defect rates or product weights) are assumed to be normally distributed to monitor consistency.

- **Survey Sampling and Polling:**  
  Sample means or proportions from large random samples are often treated as normally distributed for inference and margin-of-error calculations.

- **Financial Modeling:**  
  In finance, average returns over time are modeled using normal distributions, relying on the CLT for justification when sample sizes are large.

These examples illustrate how the CLT underpins statistical inference across disciplines, even when working with non-normal or discrete data.

## Conclusion

This report demonstrated the Central Limit Theorem through simulations using uniform, exponential, and binomial populations. The results confirmed that, as sample size increases, the distribution of sample means approaches normality — regardless of the original population shape.

These findings reinforce the CLT’s importance in statistical inference, especially in estimating population parameters and conducting hypothesis tests using real-world, non-normal data.
