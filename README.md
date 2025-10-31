# 🧬 De Novo Genome Assembly Using De Bruijn Graphs

**Reconstructing genomes from billions of fragmented DNA reads using graph theory and efficient algorithms.**

---

## 📖 Abstract
Genome assembly is a cornerstone problem in bioinformatics: reconstructing a full genome from billions of short, unordered DNA fragments ("reads"). Traditional **Overlap Graph** methods lead to an **NP-complete Hamiltonian path problem**, while **De Bruijn Graphs (DBGs)** transform it into an **Eulerian path problem**, solvable in **linear time**.  
This project implements a Python-based De Bruijn Graph construction and visualization pipeline, illustrating the key computational principles behind modern genome assembly.

---

## 🎯 The Core Problem
**String Reconstruction Problem:**  
Given a collection of $k$-mers (substrings of length *k*), reconstruct the original text.  
In genome assembly, these $k$-mers represent DNA reads obtained from sequencing machines — but appear in random order.

---

## 🔬 Methodology Overview

### 🧩 1. Overlap–Layout–Consensus (OLC)
- **Nodes:** $k$-mers (reads)  
- **Edges:** Directed if the suffix of one read overlaps with the prefix of another  
- **Goal:** Hamiltonian path visiting each node once  
⚠️ **Problem:** NP-complete — infeasible for billions of reads.

### ⚙️ 2. De Bruijn Graphs (DBG)
- **Nodes:** Unique $(k−1)$-mers  
- **Edges:** Each $k$-mer forms a directed edge from its prefix to suffix $(k−1)$-mers  
- **Goal:** Eulerian path (visits every edge once)  
✅ **Complexity:** $O(|E|)$ — efficient and scalable.

---

## 📈 Key Concepts

### Eulerian Paths & Cycles
- **Balanced Node:** $IN(v) = OUT(v)$  
- **Eulerian Cycle:** Exists if all nodes are balanced (circular genomes)  
- **Eulerian Path:** Exists if exactly one start and one end node are unbalanced (linear chromosomes)

### Handling Ambiguities
1. **Paired-End Reads:** Incorporate long-range information to resolve repeat-induced tangles.  
2. **Bubble Popping:** Removes erroneous branches caused by sequencing errors.

---

## 🐍 Implementation

**Language:** Python  
**Libraries:** `random`, `toyplot`  

### Core Functions
```python
get_kmer_count_from_sequence(sequence, k)
# Extracts all k-mers from a DNA sequence.

get_debruijn_edges_from_kmers(kmers)
# Constructs directed edges between (k−1)-mer prefixes/suffixes.

plot_debruijn_graph(edges)
# Visualizes the resulting De Bruijn graph.
```
### 🧪 Example Experiments

1.  **Binary Universal String ($k=3$)**
    * **Sequence:** `0001110100`
    * **Graph:** Complete Eulerian cycle containing all possible 2-mer nodes.
    
    * **Insight:** Demonstrates perfect reconstruction when coverage is ideal.

2.  **Random DNA Sequence (`AGATGAATGGACCGGCCATATAAGT`)**
    * **Case A: Large $k$ (= 6)**
        * Produces a single unambiguous path.
        * ✅ Unique k-mers → easy assembly.
    * **Case B: Small $k$ (= 4)**
        * Produces a tangled graph with multiple outgoing/incoming edges.
        
        * ⚠️ Ambiguities → repeat regions unresolved.

**Trade-off:**

* **Small $k$:** High coverage, low uniqueness (complex graph)
* **Large $k$:** Clear graph, risk of missing k-mers (gaps)

#### 💡 Key Takeaways

* Genome assembly complexity drops from NP-complete to linear time via De Bruijn Graphs.
* Real-world assemblers handle repeats and errors using paired-reads and bubble-popping.
* The choice of $k$ determines the balance between graph simplicity and data completeness.
