---
layout: post
title: "Project Proposal"
---

## Introduction & Background

Music streaming platforms like Spotify and SoundCloud have greatly increased the consumption of music throughout recent years. To keep users engaged with their products, these services have invested heavily into music recommendation algorithms, spurring the research domain of Music Information Retrieval (MIR).

Within MIR, researchers have investigated two central problems in music recommendations: the classification and clustering of music data. Many studies [2], [3] have already presented supervised methods for assigning music to labeled genres. However, music genres are largely subjective, leading to less accurate recommendations based on genre. Researchers have instead presented unsupervised methods [4], [5] to recommend songs with similar audio qualities without user labeling.

In this project, we explore how clustering analysis can be applied to the Million Song Dataset [1] using selected audio features like tempo.

## Problem Definition

With the advent of digital music platforms and streaming services, there is an overwhelming abundance of music available. Users often struggle to navigate through extensive catalogs to find music that aligns with their preferences and mood. We use the following data and methods to assist users in narrowing down this abundance of music to tracks that share similar characteristics to a user's currently playing songs.

## Methods

### Data

As mentioned previously, we will be using the Million Song Dataset [2], which includes audio features like the following:

- Artist familiarity
- Artist hotness
- Artist location
- Danceability
- Energy
- Key
- Loudness
- Mode
- Song hotness
- Start of fade out
- Tatums start
- Tempo
- Time signature
- Year

### Unsupervised Methods

We will be using the following methods for clustering the above dataset:

1. K-Means Clustering has been a popular choice [4], [5], [6] in clustering audio features. We will be exploring this method first in our project.
2. Gaussian Mixture Models are a more flexible alternative to K-Means Clustering.
3. Hierarchical Clustering

## Potential Results & Discussion

The absence of ground truth labels may pose a challenge in evaluating the results of our clustering models. However, there are multiple intrinsic metrics for clustering that we can use to assess the quality and performance of our algorithms: Inertia, Silhouette Coefficient, and the Davies-Bouldin Index.

Inertia describes how tight our clusters are. Ideally, we want our models to have lower inertia and lower number of clusters. The Silhouette Coefficient or Silhouette score describes how well assigned data points are to their clusters. This value ranges from -1 to 1, where a score of -1 is an indication that clusters were not assigned correctly while a score of 1 denotes that clusters are clearly distinct from each other. The Davies-Bouldin Index describes the average similarity between clusters or the distances between them relative to the size of the cluster. A lower score means your clusters are small and far from other clusters.

## References

[1] T. Bertin-Mahieux, D. P. W. Ellis, B. Whitman, and P. Lamere, "The Million Song Dataset," in Proceedings of the 12th International Society for Music Information Retrieval Conference (ISMIR 2011), 2011.

[2] Ahmet Elbir and Nizamettin Aydın, “Music genre classification and music recommendation by using deep learning,” Electronics Letters, vol. 56, no. 12, pp. 627–629, Jun. 2020, doi: https://doi.org/10.1049/el.2019.4202.

[3] Y.-H. Cheng and C.-N. Kuo, “Machine Learning for Music Genre Classification Using Visual Mel Spectrum,” Mathematics, vol. 10, no. 23, pp. 4427–4427, Nov. 2022, doi: https://doi.org/10.3390/math10234427.

[4] T. Li, Mitsunori Ogihara, W. Peng, B. Shao, and S. Zhu, “Music Clustering With Features From Different Information Sources,” IEEE Transactions on Multimedia, vol. 11, no. 3, pp. 477–485, Apr. 2009, doi: https://doi.org/10.1109/tmm.2009.2012942.

[5] P. Georges and Ngoc Cuong Nguyen, “Visualizing music similarity: clustering and mapping 500 classical music composers,” Scientometrics, vol. 120, no. 3, pp. 975–1003, Jul. 2019, doi: https://doi.org/10.1007/s11192-019-03166-0.

‌[6] Aisha Gemala Jondya and Bambang Iswanto, “Indonesian’s Traditional Music Clustering Based on Audio Features,” Procedia Computer Science, Jan. 2017, doi: https://doi.org/10.1016/j.procs.2017.10.019.
‌

## Proposed Timeline

Add proposed timeline from start to finish and list each project members’ responsibilities.

## Contributions

The following contribution table displays responsibilities that each member will tackle as we explore the above problem.

| Member       | Contributions |
| ------------ | ------------- |
| Ethan Torone | TBD           |
| Edward Chen  | TBD           |
| Brian Pak    | TBD           |
| John Pham    | TBD           |
| Vibhav Kumar | TBD           |
