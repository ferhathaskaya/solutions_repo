# Problem 1
# Equivalent Resistance Using Graph Theory

## Motivation

Calculating the equivalent resistance of a circuit is essential for understanding current flow and energy use. Traditional methods become complex with large circuits, especially when combinations of series and parallel resistors are nested.

---

## Implementation Strategy

We represent the circuit as a weighted graph using a MultiGraph structure, where:

- **Each node** is a junction in the circuit.

- **Each edge** is a resistor, with its resistance stored as an edge weight.

The simplification algorithm works by:

1. **Identifying parallel resistors** — multiple edges between the same two nodes — and replacing them with one equivalent edge using:

$$
 \frac{1}{R_{\text{eq}}} = \sum \frac{1}{R_i}
$$

2. **Identifying series resistors** — paths through intermediate nodes of degree 2 — and collapsing them into a single resistor using:
$$
   R_{\text{eq}} = R_1 + R_2 + \dots
$$

This process repeats iteratively until the graph reduces to a single equivalent resistor between the input and output terminals.

---

### Python Implementation

We implement the algorithm using the `networkx` library. The graph is simplified by:

- Merging parallel edges into one equivalent resistor.

- Collapsing series paths through non-terminal nodes.

The algorithm continues until no further reductions can be made.

<details>
<summary>Click to Expand Python Implementation</summary>

<pre><code>
```python
import networkx as nx

def simplify_circuit(G, start, end):
    G = G.copy()
    while True:
        changed = False

        # Merge parallel resistors
        for u, v in list(G.edges()):
            keys = list(G[u][v].keys())
            if len(keys) > 1:
                resistances = [G[u][v][k]['resistance'] for k in keys]
                R_parallel = 1 / sum(1 / R for R in resistances)
                G.remove_edges_from([(u, v, k) for k in keys])
                G.add_edge(u, v, resistance=R_parallel)
                changed = True
                break  # restart after modification

        if changed:
            continue

        # Collapse series nodes
        for node in list(G.nodes()):
            if node in (start, end) or G.degree(node) != 2:
                continue
            neighbors = list(G.neighbors(node))
            if len(neighbors) == 2:
                edge1 = list(G.get_edge_data(node, neighbors[0]).values())[0]
                edge2 = list(G.get_edge_data(node, neighbors[1]).values())[0]
                R_series = edge1['resistance'] + edge2['resistance']
                G.remove_node(node)
                G.add_edge(neighbors[0], neighbors[1], resistance=R_series)
                changed = True
                break

        if not changed:
            break

    # Return result
    if G.has_edge(start, end):
        return list(G.get_edge_data(start, end).values())[0]['resistance']
    else:
        return float('inf')  # no path


</code></pre>

</details>

---

## Example 1: Mixed Series and Parallel Resistors

<details>
<summary>Click to Expand Visual's Code</summary>

<pre><code>

```python
# Define graph structure
G1 = nx.MultiGraph()
G1.add_edge('A', 'B', resistance=4)
G1.add_edge('B', 'C', resistance=6)
G1.add_edge('A', 'C', resistance=12)

# Visualization function
def draw_multigraph_as_simple(G_multi, title, filename):
    G_simple = nx.Graph()
    for u, v, data in G_multi.edges(data=True):
        if G_simple.has_edge(u, v):
            existing = G_simple[u][v]['label']
            G_simple[u][v]['label'] = f"{existing} || {data['resistance']}"
        else:
            G_simple.add_edge(u, v, label=str(data['resistance']))

    pos = nx.spring_layout(G_simple, seed=42)
    edge_labels = nx.get_edge_attributes(G_simple, 'label')
    nx.draw(G_simple, pos, with_labels=True, node_color='lightblue', node_size=700, font_weight='bold')
    nx.draw_networkx_edge_labels(G_simple, pos, edge_labels=edge_labels)
    plt.title(title)
    plt.tight_layout()
    plt.savefig(f"{filename}.png")
    plt.close()

# Draw graph
draw_multigraph_as_simple(G1, "Example 1: Mixed Series and Parallel", "example1_graph_fixed")



</code></pre>

</details>

---
![alt text](<Example 1 Mixed Series and Parallel, example1_graph_fixed.png>)

**Circuit:**

A–B–C is a series path:  
- A–B = 4 Ω  
- B–C = 6 Ω

A direct A–C path also exists:  
- A–C = 12 Ω

**Interpretation:**

The 4 Ω and 6 Ω are in series → 10 Ω  
This 10 Ω is in parallel with 12 Ω


**Expected Result:**

$$
\frac{1}{R_{\text{eq}}} = \frac{1}{10} + \frac{1}{12} \Rightarrow R_{\text{eq}} \approx 5.45\ \Omega
$$


**Example 1: Step-by-Step Simplification:**

![alt text](<Example 1 – Step-by-Step Simplification.png>)

> **Fig. :**  Left: After collapsing the A–B–C series path into a single 10 Ω edge, the circuit includes two parallel resistors between A and C.
Right: These are then reduced to one equivalent resistor of 5.45 Ω, completing the simplification. 

<details>
<summary>Click to Expand Example 1: Step-by-Step Simplification Code</summary>

<pre><code>
    # Re-run after kernel reset
### Example 1 – Step-by-Step Simplification (Combined View)

```python
# Define both graphs
G_step1 = nx.MultiGraph()
G_step1.add_edge('A', 'C', resistance=10)
G_step1.add_edge('A', 'C', resistance=12)

G_final = nx.MultiGraph()
G_final.add_edge('A', 'C', resistance=5.45)

# Visualization function
def draw_combined_steps(G1, G2, titles, filename):
    fig, axes = plt.subplots(1, 2, figsize=(12, 5))
    for ax, G, title in zip(axes, [G1, G2], titles):
        G_simple = nx.Graph()
        for u, v, data in G.edges(data=True):
            if G_simple.has_edge(u, v):
                existing = G_simple[u][v]['label']
                G_simple[u][v]['label'] = f"{existing} || {data['resistance']}"
            else:
                G_simple.add_edge(u, v, label=str(data['resistance']))

        pos = nx.spring_layout(G_simple, seed=42)
        edge_labels = nx.get_edge_attributes(G_simple, 'label')
        nx.draw(G_simple, pos, with_labels=True, node_color='lightblue',
                node_size=700, font_weight='bold', ax=ax)
        nx.draw_networkx_edge_labels(G_simple, pos, edge_labels=edge_labels, ax=ax)
        ax.set_title(title)

    plt.tight_layout()
    plt.savefig(f"{filename}.png")
    plt.close()

# Create and save combined figure
draw_combined_steps(G_step1, G_final,
    ["Step 1: Series Collapsed", "Step 2: Final Equivalent"],
    "example1_combined_steps")


</code></pre>

</details>

---

## Example 2: Nested Series and Parallel Configuration

<details>
<summary>Click to Expand Visual's Code</summary>

<pre><code>

```python
# Define graph structure
G2 = nx.MultiGraph()
G2.add_edge('A', 'B', resistance=3)
G2.add_edge('B', 'C', resistance=6)
G2.add_edge('B', 'D', resistance=6)
G2.add_edge('C', 'D', resistance=6)
G2.add_edge('D', 'E', resistance=3)

# Visualization function
def draw_multigraph_as_simple(G_multi, title, filename):
    G_simple = nx.Graph()
    for u, v, data in G_multi.edges(data=True):
        if G_simple.has_edge(u, v):
            existing = G_simple[u][v]['label']
            G_simple[u][v]['label'] = f"{existing} || {data['resistance']}"
        else:
            G_simple.add_edge(u, v, label=str(data['resistance']))

    pos = nx.spring_layout(G_simple, seed=42)
    edge_labels = nx.get_edge_attributes(G_simple, 'label')
    nx.draw(G_simple, pos, with_labels=True, node_color='lightblue', node_size=700, font_weight='bold')
    nx.draw_networkx_edge_labels(G_simple, pos, edge_labels=edge_labels)
    plt.title(title)
    plt.tight_layout()
    plt.savefig(f"{filename}.png")
    plt.close()

# Draw graph
draw_multigraph_as_simple(G2, "Example 2: Nested Series and Parallel", "example2_graph")

</code></pre>

</details>

---
![alt text](<Example 2 Nested Series and Parallel, example2_graph.png>)

**Circuit:**

- A–B = 3 Ω  
- B–C = 6 Ω  
- B–D = 6 Ω  
- C–D = 6 Ω  
- D–E = 3 Ω

**Interpretation:**

- Nodes C and D form a **parallel pair** (6 Ω || 6 Ω = 3 Ω)
- B–(C,D)–E becomes a **series chain**: 6 + 3 + 3 = 12 Ω
- Final path A–B–(C,D)–D–E = 3 + 12 = 15 Ω


**Example 2 – Step-by-Step Simplification:**

![alt text](<Example 2 – Step-by-Step Simplification-1.png>)

> **Fig. :**  Left: The triangular B–C–D configuration is reduced to a single 4 Ω equivalent resistor between B and D.
Right: The resulting A–B–D–E path (3 + 4 + 3) is simplified into one 10 Ω resistor between A and E.

<details>
<summary>Click to Expand Example 2: Step-by-Step Simplification Code</summary>

<pre><code>

```python
# Step 1: Triangle B–C–D reduced
G2_step1_corrected = nx.MultiGraph()
G2_step1_corrected.add_edge('A', 'B', resistance=3)
G2_step1_corrected.add_edge('B', 'D', resistance=4)  # result of reducing B–C–D network
G2_step1_corrected.add_edge('D', 'E', resistance=3)

# Step 2: Final reduction to A–E
G2_final_corrected = nx.MultiGraph()
G2_final_corrected.add_edge('A', 'E', resistance=10)

# Reuse draw_combined_steps function
draw_combined_steps(
    G2_step1_corrected,
    G2_final_corrected,
    ["Step 1: B–C–D Triangle Reduced", "Step 2: Final Equivalent"],
    "example2_corrected_combined_steps"
)
</code></pre>

</details>

---

---
## Example 3: Complex Circuit with Multiple Loops

<details>
<summary>Click to Expand Visual's Code</summary>

<pre><code>

# Define graph structure
G3 = nx.MultiGraph()
G3.add_edge('A', 'B', resistance=2)
G3.add_edge('B', 'C', resistance=4)
G3.add_edge('A', 'C', resistance=6)
G3.add_edge('C', 'D', resistance=3)
G3.add_edge('D', 'E', resistance=5)
G3.add_edge('C', 'E', resistance=15)

# Visualization function
def draw_multigraph_as_simple(G_multi, title, filename):
    G_simple = nx.Graph()
    for u, v, data in G_multi.edges(data=True):
        if G_simple.has_edge(u, v):
            existing = G_simple[u][v]['label']
            G_simple[u][v]['label'] = f"{existing} || {data['resistance']}"
        else:
            G_simple.add_edge(u, v, label=str(data['resistance']))

    pos = nx.spring_layout(G_simple, seed=42)
    edge_labels = nx.get_edge_attributes(G_simple, 'label')
    nx.draw(G_simple, pos, with_labels=True, node_color='lightblue', node_size=700, font_weight='bold')
    nx.draw_networkx_edge_labels(G_simple, pos, edge_labels=edge_labels)
    plt.title(title)
    plt.tight_layout()
    plt.savefig(f"{filename}.png")
    plt.close()

# Draw graph
draw_multigraph_as_simple(G3, "Example 3: Complex Circuit with Loops", "example3_graph")


</code></pre>

</details>

---
![alt text](<Example 3 Complex Circuit with Loops, example3_graph.png>)

**Circuit:**

- A–B = 2 Ω  
- B–C = 4 Ω  
- A–C = 6 Ω  
- C–D = 3 Ω  
- D–E = 5 Ω  
- C–E = 15 Ω

**Interpretation:**

- A–B–C is in series (2 + 4 = 6 Ω), in parallel with direct A–C (6 Ω)
  → A–C total: 3 Ω  
- C–D–E (3 + 5 = 8 Ω), in parallel with C–E (15 Ω)
  → C–E total: \( \frac{1}{8} + \frac{1}{15} \) → ~5.22 Ω

Final total path A–C–E: 3 + 5.22 ≈ **8.22 Ω**


**Example 3 – Step-by-Step Simplification:**

![alt text](<Example 3 – Step-by-Step Simplification.png>)

> **Fig. :**  Left: The parallel paths A–B–C with A–C, and C–D–E with C–E, are reduced to single resistors.
Right: The simplified A–C–E path becomes one equivalent edge between A and E, completing the circuit reduction.

<details>
<summary>Click to Expand Example 3: Step-by-Step Simplification Code</summary>

<pre><code>
    # Re-run after kernel reset

```python
# Step-by-step graph structures
G3_step1 = nx.MultiGraph()
G3_step1.add_edge('A', 'C', resistance=3)      # result of A–B–C || A–C
G3_step1.add_edge('C', 'E', resistance=5.22)   # result of C–D–E || C–E

G3_final = nx.MultiGraph()
G3_final.add_edge('A', 'E', resistance=8.22)   # final equivalent

# Reuse draw_combined_steps() from earlier

# Draw and save combined figure
draw_combined_steps(G3_step1, G3_final,
    ["Step 1: Parallel Groups Collapsed", "Step 2: Final Equivalent"],
    "example3_combined_steps")


</code></pre>

</details>


---

## Conclusion

This project demonstrated how graph theory can simplify the analysis of electrical circuits by reducing complex resistor networks to a single equivalent resistance. Through step-by-step simplification of series and parallel combinations, we showed that graph-based methods provide a structured, visual, and automatable approach to circuit reduction.

## Efficiency and Limitations

The implemented method uses iterative detection of series and parallel resistor patterns. It performs well for small to medium-sized circuits and converges quickly in most practical cases.

- **Computational Efficiency:** Each iteration runs in \( O(n + m) \), where \( n \) is the number of nodes and \( m \) is the number of edges.
- **Limitations:** The algorithm currently supports only resistive components and cannot simplify bridge networks or circuits containing voltage/current sources.
- **Possible Extensions:** The approach could be enhanced with support for star-delta (Y–Δ) transformations, additional circuit elements, and a graphical interface for interactive analysis.
