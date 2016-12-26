# Morning Lecture

## K means

Guaranteed to converge. But it's unknown whether it will converge to the best answer.

Therefore we must run it multiple times to see what results we get.

1. The first step chooses the initial centroids, with the most basic method being to choose k samples from the dataset X.
2. After initialization, K-means consists of looping between the two other steps. The algorithm repeats these last two steps until this value is less than a threshold. In other words, it repeats until the centroids do not move significantly.
 1. The first step assigns each sample to its nearest centroid.
 2. The difference between the old and the new centroids are computed and the second step creates new centroids by taking the mean value of all of the samples assigned to each previous centroid.


# Afternoon Lecture

## Hierarchical clustering

__How to cluster: methods of linkage__

 - Complete
  * Will make more blob-y things like in k-means
 - Average
  * Higher runtime
 - Single
  * Good for geographical clustering (e.g. streets)
 - Centroid
  * Even more calculations

__Similarity metrics__

 - Euclidean
  * Good for airlines or something
 - Manhattan
  * Good for driving
 - Cosine
  * Good for text
  * Titanic should be the same as Titanic x 10
 - Jaccard
  * Purchasing history
  * Graph theory

Build the entire thing and get dendrogram. Cut off wherever you want. Decide where to cut off (i.e. decide number of k) as before (e.g. elbow, etc.)

### How do we know if our clustering is good?

 __Elbow__
  - Same as before
  - Choose the k at which error stops drastically decreasing

__Silhouette coefficient__

  - We want points to be similar to other points in the same cluster
  - And also dissimilar from other clusters

__GAP statistic__
 - Put in a bunch of fake data (evenly distributed) and then cluster that fake data
 - If the fake clusters are similar to our clusters then our clusters probably suck
