# Graph Algorithms for Wikipedia Network Analysis

This repository implements graph theory algorithms to analyze a reduced Wikipedia article network. The project demonstrates various graph analysis techniques on real-world data, exploring link structures, community detection, and centrality measures within the Wikipedia ecosystem.

## Dataset

The implementation works with a reduced Wikipedia dataset consisting of:
- Articles as vertices (stored in `vertexes.txt`)
- Hyperlinks between articles as edges (stored in `edges.txt`)
- Edge weights representing the relevance or strength of connections

The dataset format:
- Vertices: `[index] [article title]`
- Edges: `[source_article_index] [target_article_index] [weight]`

This reduced dataset maintains the complex network properties of Wikipedia while being manageable for analysis and algorithmic exploration.

## Graph Representation

The graph uses an adjacency list representation implemented as nested dictionaries:

```python
self._graph = {
    'Article A': {
        'data': article_metadata,
        'neighbors': {
            'Article B': weight_AB,
            'Article C': weight_AC,
        }
    },
    # More articles...
}
```

This structure provides:
- O(1) average-case vertex and edge lookups
- Efficient neighbor iteration
- Space-efficient storage for sparse graphs like Wikipedia's link structure

## Connectivity Analysis

### Connected Components

The algorithm identifies connected components in the graph using depth-first search (DFS):

```python
def calcular_conexidad(graph: Graph):
    # Implementation details...
```

**Theory**: Connected components are subgraphs where any vertex can reach any other vertex via some path. In the Wikipedia context, this reveals clusters of articles that are linked together but isolated from other clusters. The algorithm uses DFS to explore as far as possible along each branch before backtracking, ensuring complete exploration of each component.

### Directed vs. Undirected Analysis

The implementation includes a function to convert a directed graph to undirected, adding reciprocal edges:

```python
def convertir_no_dirigido(graph):
    # Implementation details...
```

**Theory**: While Wikipedia's link structure is inherently directed (page A links to B doesn't mean B links to A), analyzing the undirected version provides insights into the underlying topic relationships regardless of link direction. This transformation helps identify communities and measure the overall connectedness of the content network.

## Shortest Path Algorithms

### Bidirectional Breadth-First Search

```python
def caminos_minimos(graph: Graph, v1, v2):
    # Implementation details...
```

**Theory**: Bidirectional search simultaneously explores the graph from both the source and target vertices, significantly reducing the search space. When the frontiers meet, a path is found. In the Wikipedia context, this efficiently finds the shortest link path between articles, revealing how topics connect through the "degrees of separation" concept.

### Weighted Path Optimization

```python
def reconstruir_caminos2(beggining, end, graph, weight, min_max, anterior):
    # Implementation details...
```

**Theory**: In a weighted graph, the "shortest" path may not have the fewest edges but rather the minimum total weight. This algorithm finds paths that either minimize or maximize the accumulated edge weights, allowing exploration of both the strongest and weakest connection chains between Wikipedia articles.

## Network Diameter Estimation

```python
def estimar_diametro(graph: Graph):
    # Implementation details...
```

**Theory**: The diameter of a graph is the length of the longest shortest path between any pair of vertices. For large networks like Wikipedia, exact diameter computation is prohibitively expensive, so this implementation uses a sampling approach:

1. Select random starting vertices
2. Find the farthest vertex from each starting point using BFS
3. Take the maximum distance found as an estimate of the diameter

This provides insight into the "small world" property of the Wikipedia network, showing how many clicks are needed to navigate between the most distant articles.

## Centrality Measures

### Random Walk Centrality

```python
def random_walks(graph: Graph):
    # Implementation details...
```

**Theory**: This implementation approximates PageRank-like centrality by performing random walks through the graph. Vertices visited more frequently during random traversals are considered more central. This reveals which Wikipedia articles act as "hubs" in the knowledge network, similar to how Google originally ranked web pages.

### Betweenness Centrality

```python
def betweenness(graph: Graph):
    # Implementation details...
```

**Theory**: Betweenness centrality measures how often a vertex appears on shortest paths between other vertices. High betweenness indicates articles that serve as bridges between different topics or knowledge domains. Due to computational constraints, this implementation samples random pairs of vertices rather than calculating all possible paths.

## Community Detection

### Label Propagation Algorithm

```python
def label_propagation(graph: Graph):
    # Implementation details...
```

**Theory**: The Label Propagation Algorithm (LPA) is a near-linear time algorithm for community detection:

1. Initialize vertices with unique labels
2. Iteratively update each vertex's label to the most frequent label among its neighbors
3. When labels stabilize, vertices with the same label form communities

In the Wikipedia context, this reveals clusters of articles that form coherent knowledge domains or topics, without requiring predefined categories.

## Clustering Coefficient Analysis

```python
def estimar_clustering_por_muestreo(graph, sample_size=100):
    # Implementation details...
```

**Theory**: The clustering coefficient measures how vertices tend to cluster together:

1. For each vertex, calculate the proportion of possible connections between neighbors that actually exist
2. A high coefficient indicates a "cliquish" neighborhood

This implementation uses sampling to estimate the global clustering coefficient, revealing the degree to which Wikipedia articles form tightly knit groups, which helps understand knowledge organization and topic relationships.

## Applications and Insights

The algorithms implemented here provide several insights into the Wikipedia knowledge network:

1. **Knowledge Organization**: Community detection reveals how knowledge naturally organizes into domains
2. **Information Flow**: Centrality measures identify key articles that control information flow
3. **Topic Relationships**: Shortest paths show how seemingly unrelated topics connect
4. **Navigability**: Diameter estimation demonstrates the "small world" property of Wikipedia
5. **Content Gaps**: Connectivity analysis can reveal isolated content that needs better integration

## Performance Considerations

The implementation employs several techniques to handle the large Wikipedia dataset:

1. **Dictionary-based storage** for O(1) lookups
2. **Sampling approaches** for computationally intensive measures
3. **Bidirectional search** to reduce path-finding complexity
4. **Progress tracking** with `tqdm` for long-running operations
5. **Memory-efficient data structures** like sets and deques

## References

- Kleinberg, J., & Tardos, É. (2006). Algorithm Design. Pearson Education.
- Newman, M. E. J. (2010). Networks: An Introduction. Oxford University Press.
- Raghavan, U. N., Albert, R., & Kumara, S. (2007). Near linear time algorithm to detect community structures in large-scale networks. Physical Review E, 76(3).
- Brandes, U. (2001). A faster algorithm for betweenness centrality. Journal of Mathematical Sociology, 25(2), 163-177.
- Page, L., Brin, S., Motwani, R., & Winograd, T. (1999). The PageRank citation ranking: Bringing order to the web. Stanford InfoLab.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
