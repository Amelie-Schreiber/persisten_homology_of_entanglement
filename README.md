# persistent_homology_of_entanglement
This repo attempts to implements the following paper [Persistent homology of quantum entanglement](https://arxiv.org/abs/2110.10214) starting from graph states with controlled random unitary matrix gates as entangling gates between qubits. Also see [Interaction graph-based profiling of quantum benchmarks for improving quantum circuit mapping techniques](https://arxiv.org/abs/2212.06640)

Dividing the quantum circuit into time-steps and computing the reduced density matrix operator for each time step allows us to construct a weighted interaction graph, where the weights are given by the von Neumann entanglement entropy between each pair of qubits (or alternatively quantum mutual information). By considering the entanglement entropy (or the quantum mutual information) as a parameter, we obtain a bifiltration of the weighted interaction graph.

This bifiltration can be used to compute the 2-parameter persistent homology of the weighted interaction graph. The first parameter is time, and the second parameter is the entanglement entropy. The persistent homology captures the topological features of the graph that persist across different time-steps and entanglement entropy values.

The 2-parameter persistent homology of the weighted interaction graph can be used to study the evolution in "time" of the entanglement structure of the quantum circuit. This can provide insights into the quantum information processing that is happening in the circuit, and can help us understand the role of entanglement in quantum computation. This can again be utilized in understanding and improving the transpiling process, especially when hardware backend constraints are topological in nature. 

Overall, computing the 2-parameter persistent homology of a bifiltration constructed from a quantum circuit can be a useful tool for studying the entanglement structure of the circuit and gaining insights into the quantum information processing that is happening. We can then use [Rivet](https://github.com/rivetTDA/rivet). It would also be a very interesting problem both mathematically and physically to see a complete study of how these interaction graphs change based on the chosen basis gate set. 

Start by defining a quantum circuit. The ineraction graphs weighted by two-qubit gate count, von Neumann entanglement entropy, or quantum mutual inforamtion can then be obtained by using the appropriate notebook. Once the weighted interaction graph is obtained we use its weighted adjacency matrix to obtain a distance matrix. For example, we could define the distance matrix using inverse quantum mutual information as "distance", i.e. the filtration parameter for persistent homology. Given a distance matrix (or weighted adjacency matrix, we can compute one-parameter persistent homology by using the following Gudhi code:


```
import numpy as np
import matplotlib.pyplot as plt
import gudhi

# Generate a random distance matrix (should use weighted adjacency matrix from circuit)
N = 100
D = np.random.rand(N, N)
D = np.triu(D) + np.triu(D, 1).T  # make it symmetric
np.fill_diagonal(D, 0)  # set diagonal to zero

# Compute the persistence homology of the distance matrix
rips_complex = gudhi.RipsComplex(distance_matrix=D)
simplex_tree = rips_complex.create_simplex_tree(max_dimension=2)
diag = simplex_tree.persistence(homology_coeff_field=2)

# Plot the persistence diagram
gudhi.plot_persistence_diagram(diag)
plt.show()

# Plot the persistence barcode
gudhi.plot_persistence_barcode(diag)
plt.show()
```

This can then be used as a topological feature for learning to classify quantum circuits via their interaction graph. The classification can be performed with respect to ASIC or hardware backend requirements. 

Could we for example, we might want interaction graphs that can be embedded into surfaces of a particular genus to be used with a particular surface code? Might we also study probabilistic graph grammars with this, and classify interaction graphs with particular grammatical structure?
