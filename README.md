## Beamer: Beam Agglomerative Clustering
Most hierarchical clustering algorithms are greedy. At each step, they merge the closest pair of clusters according to some linkage criterion. This simple, efficient strategy has stood the test of time. But it comes with a cost: once a merge is made, there's no going back. Local decisions can lead to globally poor clusterings, especially when data have complex or noisy structure.

This post explores a modest extension: replacing greedy agglomeration with beam search. Instead of committing to a single best merge at each step, we retain a small set of promising clusterings (the "beam") and expand them in parallel. The idea is to allow limited lookahead to improve global structure.

Implementation Details

We implement beam search over hierarchical clustering with average linkage as the merge cost. At each iteration:

We track the top k partial clusterings (beam width = 3 by default).

Each clustering generates candidate merges by pairing clusters and computing their average linkage.

The lowest-cost merges are used to generate new partial clusterings.

We retain the top k resulting clusterings for the next round.

The process continues until we reach the desired number of clusters. Final cluster assignments are compared against ground truth labels using Adjusted Rand Index (ARI).

Empirical Evaluation

We test beam search agglomerative clustering on seven datasets: four synthetic (moons, circles, blobs, classification) and three real (Iris, Wine, Digits). Standard agglomerative clustering (single linkage) serves as the baseline.

Beam search performs comparably or better on several harder datasets, particularly those with noise or weak signal. For example, on the Wine and Digits datasets, standard methods fail to recover meaningful clusters, while beam search recovers partial structure. On more separable datasets like Blobs, standard greedy methods still dominate.
