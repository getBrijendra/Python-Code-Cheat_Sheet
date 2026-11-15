
In Vector Databases, **distance metrics** measure how similar or different two vectors are.

When you convert documents into embeddings, you get numerical vectors. The database uses distance metrics to find vectors closest to your query vector.

Let's understand the important ones.

# **1. Major Vector Distance Metrics**

## **A. Euclidean Distance (L2)**

Measures the straight-line distance between two points in vector space.

Formula:

\\[ d(A,B)=\sqrt{\sum\_{i=1}^{n}(A_i-B_i)^2} \\]

Example:

```
A = [1, 2]
B = [4, 6]

Distance = √((4-1)² + (6-2)²)
         = √(9 + 16)
         = 5
```

[anatomiesofintelligence.github.io](https://anatomiesofintelligence.github.io/posts/2019-09-24-euclidean_distance?utm_source=chatgpt.com)

- Smaller distance = More similar.
- Considers the magnitude and direction of vectors.
- Useful when actual geometric distance matters.

## **B. Manhattan Distance (L1)**

Measures distance by adding the absolute differences of each dimension.

Formula:

\\[ d(A,B)=\sum\_{i=1}^{n}|A_i-B_i| \\]

Example:

```
A = [1, 2]
B = [4, 6]

Distance = |4-1| + |6-2|
         = 3 + 4
         = 7
```

[K-Nearest Neighbor (KNN) Algorithm | by Palak Bhandari | Medium](https://images.openai.com/static-rsc-4/VUjJ3_CyBSTHUJ6OGwicxwiGChPn31DAW6yRh3h1-j3WUoQiUl12euElT2MuXCvtqdIIywBdWuI5xJMNg9llkl6ngNHu05mzThBnIrqIDfZjjQ5V4QrYpCoSq8trawoi0HYJpc4ja5MlTvX23PkOee9JJefexWCYvidNR-bBOxU?purpose=inline)

[medium.com](https://medium.com/%40palakvb02/k-nearest-neighbor-knn-algorithm-faa867191325?utm_source=chatgpt.com)

- Moves along individual dimensions.
- Useful in some high-dimensional and sparse-data applications.

## **C. Cosine Similarity / Cosine Distance**

Measures the angle between two vectors rather than their actual geometric distance.

Formula:

\\[ \text{Cosine Similarity} = \frac{A\cdot B}{\\|A\\|\\|B\\|} \\]

Cosine distance is commonly defined as:

\\[ d\_{\text{cosine}}=1-\text{Cosine Similarity} \\]

Example:

```
A = [1, 2]
B = [2, 4]

Cosine Similarity = 1
Cosine Distance   = 0
```

Both vectors point in the same direction.

[What is a Vector Database? Essential Guide for GenAI Developers](https://images.openai.com/static-rsc-4/E9V-rAA9DmaOmAyZ_29pds8ZpKWz_VNpxLnpQZBof_-XP8bYTG3gSA1tTm_Ek42whw8cg27uU5soq1SHy36HSx9zRcRjrjFPAFsaT89lbd7geBQRKhG6ua6t-_AZ4A5Ga5JNhIytcZhKC3W6urANZKLphqgmvbREhB_ZD-pYm1k?purpose=inline)

[psitrontech.com](https://psitrontech.com/blog/vector-store-for-generative-ai?utm_source=chatgpt.com)

- Smaller cosine distance = More similar.
- Focuses on direction, not magnitude.
- Very popular for text embeddings and semantic search.

## **D. Dot Product (Inner Product)**

Multiplies corresponding vector dimensions and adds the results.

Formula:

\\[ A\cdot B=\sum\_{i=1}^{n} A_iB_i \\]

Example:

```
A = [1, 2]
B = [3, 4]

Dot Product = (1×3) + (2×4)
            = 11
```

- Higher dot product generally means greater similarity when ranking by maximum inner product.
- Considers both direction and magnitude.
- Common in embedding models and recommendation systems.

**Important:** Dot product is a similarity score, not a mathematical distance metric in the strict sense.

# **2. Other Distance Metrics**

| Metric             | Concept                                   | Common application              |
| ------------------ | ----------------------------------------- | ------------------------------- |
| Hamming Distance   | Counts differing positions                | Binary vectors                  |
| Jaccard Distance   | Measures dissimilarity between sets       | Set/binary data                 |
| Haversine Distance | Distance between geographical coordinates | GPS/location                    |
| Minkowski Distance | Generalized L1 and L2 distance            | Mathematical/generalized search |
| Canberra Distance  | Normalized differences                    | Numerical data                  |

# **3. Quick Comparison**

| Metric            | Better result                      |
| ----------------- | ---------------------------------- |
| Euclidean (L2)    | Lower                              |
| Manhattan (L1)    | Lower                              |
| Cosine Distance   | Lower                              |
| Cosine Similarity | Higher                             |
| Dot Product       | Higher (for maximum inner product) |
| Hamming           | Lower                              |
| Jaccard Distance  | Lower                              |

# **4. Which one should you use in RAG?**

For semantic search in a typical RAG application:

| Metric            | Recommendation                                          |
| ----------------- | ------------------------------------------------------- |
| Cosine Similarity | Very common choice                                      |
| Euclidean (L2)    | Good when appropriate for embedding model               |
| Dot Product       | Good when model is trained for inner-product similarity |

**Important:** The best metric depends on how the embedding model was trained. Don't assume cosine is always better.

For example, if embeddings are normalized to unit length:

\\[ \\|A\\|=\\|B\\|=1 \\]

Then:

\\[ L2^2=2(1-\text{Cosine Similarity}) \\]

This means cosine similarity and Euclidean distance produce the same ranking for normalized vectors.

# **5. Python Example**

```
import numpy as np

A = np.array([1, 2, 3])
B = np.array([2, 3, 4])

# Euclidean distance
l2 = np.linalg.norm(A - B)

# Manhattan distance
l1 = np.sum(np.abs(A - B))

# Cosine similarity
cosine = np.dot(A, B) / (
    np.linalg.norm(A) * np.linalg.norm(B)
)

# Dot product
dot = np.dot(A, B)

print("L2 Distance:", l2)
print("L1 Distance:", l1)
print("Cosine Similarity:", cosine)
print("Cosine Distance:", 1 - cosine)
print("Dot Product:", dot)
```

**Interview tip:** Remember these four first:

- L2 → Straight-line distance.
- L1 → Sum of absolute differences.
- Cosine → Angle/direction similarity.
- Dot Product → Direction and magnitude together.

These are the most important metrics to understand when working with vector databases such as Qdrant, Chroma, Milvus and pgvector.



----------------------------------------------------------------------------------------------------------------------


there are several algorithms used for indexing and searching high-dimensional vectors in Vector Databases.

They are generally called **ANN (Approximate Nearest Neighbor) algorithms**.

The objective is the same: find vectors similar to a query vector without comparing it against every stored vector.

## **1. Major Vector Search Algorithms**

![Retrieval Augmented Generation (RAG) 08: Advanced Indexing Methods for Vector Search | by Yashwanth S | Medium](https://images.openai.com/static-rsc-4/GaDF2hdFjzH4wdoXqBEceOsKi4a9XV64fm3J3UI9RkyaTPGrDJyeusF37D_zbkomD_NTgvzFLobRRzwUynRCXOQ8pTY1xKlH91w028T2FFHX9DLkvHyACR8MeRwO0-5xMN0UtyJgb2aBPESPfLazDTnhAVsfjNndmQnfhyen04U?purpose=inline)

**1. HNSW (Graph-based)**

- Creates a multi-layer graph.
- Navigates between connected vectors.
- Fast search with high recall.
- Trade-off: high memory consumption.

Popular choice

![IVF Index: Clustering-Based Vector Search & Partitioning - Interactive | Michael Brenndoerfer | Michael Brenndoerfer](https://images.openai.com/static-rsc-4/9qRuFpmfqVuqWLzPBL49n8ns2oK1pb0QkswY5urM7HzqtSYUlV2nn_I35GIfnhweX5hH0LPghUniaVHAO_JJDk6A3bYknk1CzIQwOsMy7jh6x51Q-E_tK3CLp1-W_Ump4o2IdDYu4e_vFFV6KH3iQEciJMYRPfkkxKqUJmJrKEg?purpose=inline)

**2. IVF (Inverted File Index)**

- Groups vectors into clusters.
- Each cluster has a centroid.
- Search first identifies relevant clusters.
- Searches only vectors in selected clusters.

Good for large datasets.

![Retrieval Augmented Generation (RAG) 08: Advanced Indexing Methods for Vector Search | by Yashwanth S | Medium](https://images.openai.com/static-rsc-4/IFCQh7vhMXR1YwqQh6ph2v6agmpyx-nWF2FB1EbM_31B-9pGlAB9FkMNCgAXhF6KPkdE0Zuu-pSXprLqtJMkocF73WSLKlf4IsiVgyxG5vcxtiU0-nhjUTQ_QfVVro5a3_lMYT3_We4XayjuA7AKfSvWH_S0hvOQ9Qg4jtOEcFs?purpose=inline)

**3. PQ (Product Quantization)**

- Compresses high-dimensional vectors.
- Divides vectors into smaller subspaces.
- Stores compact representations.
- Reduces memory consumption.

Useful for very large datasets.

![ANN Search Explained | Cloud | Zilliz Cloud Developer Hub](https://images.openai.com/static-rsc-4/BGExfxvPO1ry-Pat165rgafl7Ip0VFVaB3Kfs_SkxZKm-5XDM_PJQHf8_WEsreGwCN-um6WjavIpVBj_FYdddJCQwo8lpZ6NoC4E8x-jR1VhTzMX86w7Nwivpo3ifxV9hR6_yp6EzJ0BcRcZg68u37Xk-of3d6HM3_F23o80g0Q?purpose=inline)

**4. LSH (Locality Sensitive Hashing)**

- Converts vectors into hash values.
- Similar vectors tend to land in the same buckets.
- Searches relevant buckets instead of all vectors.

Useful for approximate similarity search.

![Vector Search Explained: From ENN to HNSW — A Complete Guide to Similarity Search | by Amit | Medium](https://images.openai.com/static-rsc-4/IWHr-iirCbC1wfBxLVW1-Yrht-c6k-Z6eIqtGkyRj7yZdnAiyqdgshvo6Pidtep6sB4m3gPDjETm8Ebmuku2b5GKZmujdIxNiv9qb8B1VsLjimOA0sXny5RTrQWyb3xTO_5Aitv37y8VNg6F15yQ28VpTL7OiGC2qgAtHlsvwxI?purpose=inline)

**5. Annoy (Approximate Nearest Neighbors Oh Yeah)**

- Builds multiple random projection trees.
- Each tree partitions vector space.
- Combines candidate results from multiple trees.

Developed by Spotify.

![Find nearest neighbor using KD Tree | kanoki](https://images.openai.com/static-rsc-4/ml3zilD-xY9HhfyYMy1SzcJnLk_TkH2KCopT-umq0lbpaOpqtCAw3_6BtpNskmoagpeBAcDamZBu5-VQ46qmxUCWl38fvLVVU2BI1YSh6_gajNxkagDuOecWU3wggavPWvxb1tVEavHmg_7TYDGjG78g4WEjXgjQYExCu4XQPWg?purpose=inline)

**6. KD-Tree**

- Divides space into regions using dimensions.
- Uses a tree structure to search.
- Works well with low-dimensional data.
- Performance degrades with high dimensions.

## **2. Comparison**

| Algorithm | Data structure            | Main advantage           |
| --------- | ------------------------- | ------------------------ |
| HNSW      | Multi-layer graph         | Fast search, high recall |
| IVF       | Clusters / inverted lists | Scalable search          |
| PQ        | Compressed vectors        | Memory efficiency        |
| LSH       | Hash tables               | Approximate similarity   |
| Annoy     | Random projection trees   | Read-heavy workloads     |
| KD-Tree   | Binary spatial tree       | Low-dimensional search   |

## **3. Which Vector DB uses which algorithm?**

| Vector DB | Supported indexing                  |
| --------- | ----------------------------------- |
| pgvector  | HNSW, IVFFlat                       |
| Chroma    | HNSW-based indexing                 |
| Qdrant    | HNSW                                |
| Milvus    | HNSW, IVF, FLAT, DiskANN and others |
| Weaviate  | HNSW, Flat, dynamic index           |
| FAISS     | Flat, IVF, PQ, HNSW, and others     |

## **4. Important: Exact vs Approximate Search**

There are two broad approaches:

**A. Exact search (Brute Force / FLAT)**

```
Query Vector
     |
     v
Compare against ALL vectors
     |
     v
Calculate distances
     |
     v
Return exact nearest neighbors
```

- 100% exact nearest-neighbor results.
- Expensive for large datasets.

**B. Approximate search (ANN)**

```
Query Vector
     |
     v
Use an index (HNSW / IVF / LSH)
     |
     v
Search selected candidates
     |
     v
Return approximate nearest neighbors
```

- Much faster for large datasets.
- May miss some actual nearest neighbors.

## **5. What should you learn first?**

For practical AI/RAG application development, I recommend this order:

1. **HNSW** – Understand graph-based vector indexing.
2. **IVF** – Understand clustering-based indexing.
3. **PQ** – Understand vector compression.
4. **Flat search** – Understand exact similarity search.
5. **DiskANN** – Understand large-scale, disk-based vector search.

**My recommendation:** Focus on HNSW, IVF and Flat first. These three will give you a strong understanding of how vector databases retrieve similar documents.



# **Vector Database Indexing Algorithms – Short Notes**

## **1. HNSW – Hierarchical Navigable Small World**

Graph-based

**Concept:** Creates a multi-layer graph connecting similar vectors. Search navigates through the graph instead of checking every vector.

**How it works:**

1. Creates multiple graph layers.
2. Top layers contain fewer nodes for faster navigation.
3. Moves towards more similar vectors.
4. Bottom layer performs detailed candidate search.

**Important parameters:**

- `M`: Maximum graph connections.
- `ef_construction`: Search effort during index building.
- `ef_search`: Search effort during querying.

**Advantages:** Fast search, high recall.

**Disadvantages:** High memory usage and index-building cost.

**Used by:** Qdrant, Chroma, pgvector, Weaviate.

## **2. IVF – Inverted File Index**

Cluster-based

**Concept:** Groups vectors into clusters using clustering algorithms such as K-Means. Search happens only in selected clusters.

**How it works:**

1. Divides vectors into clusters.
2. Calculates a centroid for each cluster.
3. Finds the nearest centroids for the query.
4. Searches vectors inside selected clusters.

**Important parameters:**

- `nlist`: Number of clusters.
- `nprobe`: Number of clusters searched during querying.

**Advantages:** Reduces search space; suitable for large datasets.

**Disadvantages:** Can miss relevant vectors if the correct cluster isn't searched.

**Used by:** FAISS, Milvus, pgvector (IVFFlat).

## **3. ScaNN – Scalable Nearest Neighbors**

Partitioning + Quantization

**Concept:** Google's vector search technology combining partitioning, vector compression and candidate rescoring.

**How it works:**

1. Partitions vectors into clusters.
2. Searches relevant partitions.
3. Uses quantization to compress vectors and estimate distances efficiently.
4. Rescores promising candidates for better accuracy.

**Important concepts:**

- Partitioning: Reduces search space.
- Anisotropic quantization: Compresses vectors while preserving important similarity information.
- Rescoring: Improves final result quality.

**Advantages:** Efficient search and memory usage at large scale.

**Disadvantages:** More complex indexing and tuning.

**Used by:** Google ScaNN library, Google Cloud AlloyDB and ScaNN-based vector indexing services.

## **4. ACORN – Filtered Graph Search**

Filtered HNSW search

**Concept:** Improves graph-based vector search when metadata filters are applied.

**Problem with normal HNSW:**

Suppose you search for similar documents but apply a filter:

```
category = "Technology"
```

HNSW might encounter many neighboring vectors that don't satisfy the filter, making navigation inefficient.

**How ACORN works:**

1. Uses a graph-based search structure.
2. Explores neighboring nodes.
3. Also explores neighbors of neighbors.
4. Finds relevant candidates satisfying metadata filters.

**Advantages:** Better filtered ANN search, particularly with restrictive filters.

**Disadvantages:** Additional search overhead and tuning requirements.

**Used by:** Qdrant (ACORN search strategy).

## **5. Quick Comparison**

| Feature       | HNSW            | IVF               | ScaNN                    | ACORN                                     |
| ------------- | --------------- | ----------------- | ------------------------ | ----------------------------------------- |
| Approach      | Graph           | Clustering        | Partition + quantization | Filtered graph                            |
| Search        | Graph traversal | Selected clusters | Selected partitions      | Graph traversal with extended exploration |
| Memory        | High            | Moderate          | Efficient                | Depends on graph                          |
| Main strength | Fast retrieval  | Large datasets    | Scalable retrieval       | Metadata filtering                        |
| Main use      | General ANN     | Large-scale ANN   | Large-scale ANN          | Filtered ANN                              |

## **6. Remember with this analogy**

[NASA SVS | Great Zoom into Atlanta, GA: The State Capitol Building](https://images.openai.com/static-rsc-4/JsfZ2L5GfD42KXiCuQGzbVbAB1Cvn_o6AchPmpepA_h7H2Jssou6oMIQMMR_ksxQzJicumCpcDnpbVQKqk7-ZfzA6LGwZ8wwiS-9-3C9h1fmZLU3yn-O2EeIiF48i0SxrlnE8zysLEDuVz2BFJspXORrGq-rm3waG05OVMO3alA?purpose=inline)

**HNSW = Road network**

Navigate through connected roads to reach the destination.

![At the core of inventory policies, you want to know how much and when you need to make an order. | Nicolas Vandeput](https://images.openai.com/static-rsc-4/RttHeAonBEaPgCGUqCp7Hto1wq9aiJ3WC23HznosxtuFB2t-c7_07FfeTqdPluCVgHD3znIuOvoD2eQ2R5_GnY4pVUO8N_zQLZT3PEyzCGwbZkF0FFq9CSwCk6GLVRqEJNVoTtkw84F87IoYDxWrb1qcrxEAXQOkuqil02COmLE?purpose=inline)

**IVF = Warehouse sections**

Find the correct section, then search inside it.

![Google's TurboQuant: AI Memory Compression Breakthrough Explained](https://images.openai.com/static-rsc-4/lrYfeOoPSuwP6dgUXfoVOzOY7d3uCOWpNJk9hMC9iXcnFpvM3y3DY48QydIHyhktuAIHQqwGkUCEBq4ESwIJnjNglr6dt9cv8P4hfYQOWiCFF7HnJSHVI36zTqz7wYVZN5VFylSoNrLTIWmk1EfWK5yE-tT0BYM0PkzNhgTvaIs?purpose=inline)

**ScaNN = Smart compressed search**

Search selected sections using compressed representations, then refine results.

![Genrefying a Non-Fiction Collection](https://images.openai.com/static-rsc-4/Ew8Gc-ch1daDMB4x7GUBb6Y2JzFIf4kUVcR3CIZsVgjsqo0A8Z3NmZ4oZZP52k2BFQt1_nSNVnbaMJtDrTlUJhe8WtMh8QbGtpF0y77HeLZKP7-vLBPyC9Jh3eg4hzXRk3UW3q5lxy7mZWYSDd1n68jPiNHWZpj-tj2J9ncVmnM?purpose=inline)

**ACORN = Filter-aware navigation**

Find relevant items while respecting category restrictions.

**Interview takeaway:**

- HNSW → Graph-based ANN.
- IVF → Cluster-based ANN.
- ScaNN → Partitioning + quantization + rescoring.
- ACORN → Efficient graph search with restrictive metadata filters.

**Important distinction:** HNSW, IVF and ScaNN are different indexing/search approaches. ACORN is specifically designed to improve filtered graph-based ANN search.

