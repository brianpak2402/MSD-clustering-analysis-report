---
layout: post
title: "Project Midterm Report"
---

## Introduction & Background

Music streaming platforms like Spotify and SoundCloud have greatly increased the consumption of music throughout recent years. To keep users engaged with their products, these services have invested heavily into music recommendation algorithms, spurring the research domain of Music Information Retrieval (MIR).

Within MIR, researchers have investigated two central problems in music recommendations: the classification and clustering of music data. Many studies have already presented supervised methods like CNNs [2, 3] to assign music tracks to a collection of labeled genres. Other common genre classifiers include Support Vector Machines (SVMs) and K-Nearest Neighbor classifiers (KNNs). However, music genres are largely subjective, and they vary depending on the listener's musical background, especially with names like “Classical Rock” and “Hardcore Punk”. Researchers have instead pursued unsupervised methods to recommend songs with similar audio qualities without user labeling. For example, Georges and Nguyen [5] use K-Means and Hierarchical clustering to group works from over 500 European classical composers. Other researchers like Li et al. [4] propose new clustering methods like bimodal clustering to assist with Music IR tasks.

In this project, we explore how clustering analysis can be applied to the [Million Song Dataset](http://millionsongdataset.com/) [1], a collection of audio-features and metadata for over 1 million contemporary music tracks collected by the Echo Nest. This dataset consists (i.e. loudness, start of fade out, end of fade in, duration), musicality (time signature, beat number, key, tempo), and popularity (i.e. song and artist popularity). Due to computational limitations, however, we will use a [subset of 10,000 songs](https://github.com/subha5gemini/MillionSongDataset/blob/master/subset-compiled.csv) from this dataset [6].

## Problem Definition

With the advent of digital music platforms and streaming services, there is an overwhelming abundance of music available for streaming. Users often struggle to navigate through extensive catalogs and playlists to find music that best aligns with their current music preferences and moods. As mentioned previously in our introduction, we cannot rely on genre to suggest similar songs due to its subjective nature. Therefore, we will be applying clustering algorithms like K-Means, GMM, & Hierarchical Clustering to see if there are any metadata attributes other than genre that can relate songs. If such attributes exist, we would also want to identify which features we can use to assist listeners finding similar music tracks.

## Methods

### Data-Cleaning

We began our data-cleaning pipeline by inspecting the first few rows of the uncleaned data to understand the structure. Our goal was to identify which attributes in this dataset would be of most benefit to our analysis. We accomplished this by removing unnecessary data.

The Echo Nest, the organization responsible for maintaining the Million Song Dataset, includes audio features like segments and bars. These attributes contain lengths for the units of audio analysis that follow a modified version of the MIDI time format. For the purpose of producing results in a timely manner, we removed these attributes from our dataset. String based columns like “artist name” and “release” would have unnecessary characters. They were wrapped by quotations and prepended by the letter “b,” and contained stop words like “A” and “I” that do not provide much significance. To remove all of this, we applied a custom text processing function that converted text to lowercase, removed punctuation, and filtered out stop words. We also inspected the number of missing values in our data. For artist familiarity, we replaced the four points that were missing a value with the median artist familiarity value. We also decided to drop columns such as “energy” and “danceability” since they had not been analyzed by the Echo Nest and therefore contained no values.

We inspected the confidence values for all data points. The Million Song Dataset includes confidence measures for attributes like time signature. Due to the nature of these attributes, it’s possible that some of the data points have confidence values close to zero, suggesting that the corresponding values were estimated rather than accurately measured. We removed any data points with a confidence value less than a threshold of 0.2 for time signature, mode, and key. We also removed any songs with no duration, tempo, or time signature.

Finally, we removed roughly 341 outliers that contained any feature value that was over 3 standard deviations away from its respective mean. We finished our data cleaning process by separating the dataset into two separate CSV files: one for numerical data and the other for categorical data.

### Pre-processing

First, we removed the “title” column from both datasets as it was only necessary for identifying data points. The first step in pre-processing that we took was for our numerical data. Using “StandardScaler” to standardize all the numerical data. This is an important pre-processing step because without standardization, features with larger scales can disproportionately influence the model. After preprocessing was completed for the numerical data, we used the elbow method for determining the optimal number of clusters. We fitted our model for pairwise feature clustering, and we evaluated these clusters.

Pre-processing for categorical data included TF-IDF matrices and LSA dimensionality reduction to process the textual information in our data such that the models can actually understand them, as our clustering models cannot interpret textual information directly. Term Frequency-Inverse Document Frequency vectorization is used to evaluate the importance of a word in a dataset. When you complete the TF-IDF vectorization (TfidfVectorizer in the sklearn library), the result is a matrix where each row represents a string in the dataset and the column represents a unique word. The cells are given a unique score which reflects the importance of the word. We then used Latent Semantic Analysis (LSA) to reduce the features of the TF-IDF matrix. LSA uses Singular Value Decomposition, a technique that decomposes a matrix into more matrices, which reduces the number of features in the matrix while still holding the most significant information that was calculated by TF-IDF vectorization. Then, based on our variPance threshold of 95%, we calculated the cumulative explained variance, which is the sum of the singular values that we got from performing SVD. The singular values represented the different features of the vectorized data. Now that we processed the data, we concatenate the reduced text data and we make sure that the most informative aspects of our text data like “artist name” and “release” are retained from the old data. The new concatenated dataset will hold the most important information, which we will standardize such that we can analyze the data, finding optimal clustering and evaluating clusters using scores from various metrics to find which pairs cluster most effectively. We will discuss how we analyzed our data more in-depth in the Results & Discussion section.

### Unsupervised Methods

To determine if there are any metadata attributes that best relate songs, we perform clustering on all pairs of features available to us from the cleaned dataset. We can input feature vectors of these feature pairs into algorithms like K-Means, GMM, and Hierarchical clustering to produce clusters in a 2D space. These clusters can be easily evaluated using internal measures to determine which features, if any, produce stronger relationships between song tracks.

To determine which feature pairs produce the best clusterings, we first perform the following internal metrics on each clustering using various metrics depending on the method: Davies-Bouldin Index, Silhouette Coefficient, Inertia, Calinski-Harabasz Index, Dunn Index, and Cophenetic correlation. We assign rankings to each feature-pair clustering in each of the above internal metrics, and we average the ranking across all metrics. We discuss this metric further in the following section about K-Means.

## Results & Discussion

### K-Means

The first clustering algorithm we applied was K-Means clustering. To find the optimal number of clusters, we utilized the elbow method using inertia and decided on 10 to be the best K.

![KMeans Elbow Method](/music-clustering-report/assets/images/KM%20Elbow%20Method.png)

After that, we performed pairwise clustering on all possible pairs of features of our numerical attributes to determine which features work best for clustering audio tracks. This also allows us to visualize the data clustering easier. To evaluate the pairwise clusterings, we used multiple internal measures: Davies Bouldin Index, Silhouette Score, Inertia, Calinski-Harabasz Index, and Dunn Index.

**Visualizations of All Feature-Pairwise Clusterings for Numerical Attributes**
![KMeans Clustering pt1](/music-clustering-report/assets/images/KM%20Clusterings1.png)
![KMeans Clustering pt2](/music-clustering-report/assets/images/KM%20Clusterings2.png)
![KMeans Clustering pt3](/music-clustering-report/assets/images/KM%20Clusterings3.png)

**Visualizations of Internal Measures of the Feature-Pairwise Clustering of Numerical Attributes**
![KMeans DBS](/music-clustering-report/assets/images/KM%20DBS.png)
![KMeans Silhouette](/music-clustering-report/assets/images/KM%20Silhouette.png)
![KMeans Inertia](/music-clustering-report/assets/images/KM%20Inertia.png)
![KMeans KHS](/music-clustering-report/assets/images/KM%20KHS.png)
![KMeans Dunn](/music-clustering-report/assets/images/KM%20Dunn.png)

Next, we carried out the same procedure for the string data. Using the elbow method with inertia to find the optimal number of clusters and clustering data for pairwise features.
![KMeans Elbow Method String Data](/music-clustering-report/assets/images/KM%20Elbow%20Method%20String.png)
![KMeans Clustering String Data](/music-clustering-report/assets/images/KM%20String%20Clustering.png)
Since we are using multiple internal measures, there lies inconsistencies on how well features are paired/clustered in comparison to others. For example, according to Silhouette Score, end of fade in/start of fade out is the second best feature pair for clustering but if we look at the Davies Bouldin visualization, that one says duration/tempo is the second best clustered. In other words, the ranking of feature pairs differ depending on the metric.

To remedy this, we assigned a score/rank for each metric and then gave an average ranking across all metrics. For a given metric, the pair with the best clustering gets a score of 15. The pair with the second best clustering gets a 14 and so on. We then calculate the average across the scores to get the average rankings based on how well the pairs cluster.

![KMeans Rankings Chart](/music-clustering-report/assets/images/KM%20Rankings%20Chart.png)

**Feature Pairs sorted by How Well They Cluster:**

| Feature Pair                             | Score |
| ---------------------------------------- | ----- |
| `end_of_fade_in`/`loudness`              | 1.0   |
| `artist_familiarity`/`end_of_fade_in`    | 2.0   |
| `artist_familiarity`/`loudness`          | 3.0   |
| `artist_familiarity`/`tempo`             | 4.0   |
| `end_of_fade_in`/`tempo`                 | 5.0   |
| `loudness`/`tempo`                       | 6.0   |
| `artist_familiarity`/`start_of_fade_out` | 7.0   |
| `artist_familiarity`/`duration`          | 8.0   |
| `end_of_fade_in`/`start_of_fade_out`     | 9.0   |
| `duration`/`end_of_fade_in`              | 10.0  |
| `duration`/`loudness`                    | 11.0  |
| `loudness`/`start_of_fade_out`           | 12.0  |
| `start_of_fade_out`/`tempo`              | 13.0  |
| `duration`/`tempo`                       | 14.0  |
| `duration`/`start_of_fade_out`           | 15.0  |
| `artist_name`/`release`                  | 16.0  |

Looking at the results, we can see that songs are best clustered by artist name and release (album name), which makes intuitive sense. The best numerical feature pair is duration and start of fade out. The worst was the end of fade in and loudness.

### Hierarchical Clustering

We next applied hierarchical clustering. Using the elbow method with silhouette score as our metric, we decided on 20 as our optimal number of clusters.

![HC Elbow Method](/music-clustering-report/assets/images/HC%20Elbow%20Method.png)

We found all possible pairs of the numerical variables, and performed hierarchical clustering on each pair. This allowed us to easily visualize the clustering as well as determine which pairs of variables do the best job at creating clusters.

**Visualizations of Hierarchical Clusterings**
![HC Clusterings pt1](/music-clustering-report/assets/images/HC%20Clusterings1.png)
![HC Clusterings pt2](/music-clustering-report/assets/images/HC%20Clusterings2.png)
![HC Clusterings pt3](/music-clustering-report/assets/images/HC%20Clusterings3.png)
![HC Clusterings pt4](/music-clustering-report/assets/images/HC%20Clusterings4.png)
![HC Clusterings pt5](/music-clustering-report/assets/images/HC%20Clusterings5.png)

To evaluate the performance of each pair of variables, we used the following metrics: Silhouette score, Davies-Bouldin index, and Cophenetic coefficient. The following visualizations are the results for all pairs of variables.

**Silhouette Score:**
![HC Silhouette](/music-clustering-report/assets/images/HC%20Silhouette.png)

**Davies-Bouldin Index:**
![HC DBS](/music-clustering-report/assets/images/KM%20DBS.png)

**Cophenetic Coefficient:**
![HC CPC](/music-clustering-report/assets/images/HC%20CPC.png)

Since performance of a given pair of variables can vary depending on the metric used, we assigned each pair of variables a rank/score that denotes how well it did compared to other pairs in a given metric. For example, if a pair has the highest Silhouette score, then it would have a score of 15 for this metric since it performed the best. We then took the average of all these ranks across the three metrics to produce a general score to have a sense of which pairs performed best overall. The following table is the ranking of all the pairs:

| Feature Pair                             | Score |
| ---------------------------------------- | ----- |
| `artist_familiarity`/`duration`          | 13.33 |
| `artist_familiarity`/`tempo`             | 12.33 |
| `artist_familiarity`/`start_of_fade_out` | 10.67 |
| `duration`/`start_of_fade_out`           | 10.67 |
| `end_of_fade_in`/`tempo`                 | 9     |
| `loudness`/`start_of_fade_out`           | 8.67  |
| `duration`/`end_of_fade_in`              | 8.67  |
| `artist_familiarity`/`loudness`          | 8     |
| `artist_familiarity`/`end_of_fade_in`    | 7.67  |
| `loudness`/`tempo`                       | 6.67  |
| `duration`/`loudness`                    | 6.67  |
| `end_of_fade_in`/`loudness`              | 5.67  |
| `start_of_fade_out`/`loudness`           | 3.33  |
| `duration`/`tempo`                       | 1     |

From hierarchical clustering, artist familiarity/duration performs the best, and it seems that artist familiarity is an important variable to cluster on in general.

### Gaussian Mixture (GMM)

The next clustering algorithm we used was Gaussian Mixture Models. To find the optimal number of components, we utilized the elbow method using Akaike Information Criterion to find the best`K`.

![GMM Elbow Method](/music-clustering-report/assets/images/GMM%20Elbow%20Method.png)

After that, we performed pairwise clustering on all possible pairs of features of our numerical attributes to determine which features work best for clustering audio tracks. This also allows us to visualize the data clustering easier. To evaluate the pairwise clusterings, we used multiple internal measures: Davies Bouldin Index, Silhouette Score, Calinski-Harabasz Index, and Dunn Index.

**Visualizations of All Feature Pairwise Clusterings for Numerical Features:**

|                                                                                     |                                                                                     |                                                                                     |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![GMM Clustering1](/music-clustering-report/assets/images/GMM%20Clusterings11.png)  | ![GMM Clustering2](/music-clustering-report/assets/images/GMM%20Clusterings12.png)  | ![GMM Clustering3](/music-clustering-report/assets/images/GMM%20Clusterings13.png)  |
| ![GMM Clustering4](/music-clustering-report/assets/images/GMM%20Clusterings21.png)  | ![GMM Clustering5](/music-clustering-report/assets/images/GMM%20Clusterings22.png)  | ![GMM Clustering6](/music-clustering-report/assets/images/GMM%20Clusterings23.png)  |
| ![GMM Clustering7](/music-clustering-report/assets/images/GMM%20Clusterings31.png)  | ![GMM Clustering8](/music-clustering-report/assets/images/GMM%20Clusterings32.png)  | ![GMM Clustering9](/music-clustering-report/assets/images/GMM%20Clusterings33.png)  |
| ![GMM Clustering10](/music-clustering-report/assets/images/GMM%20Clusterings41.png) | ![GMM Clustering11](/music-clustering-report/assets/images/GMM%20Clusterings42.png) | ![GMM Clustering12](/music-clustering-report/assets/images/GMM%20Clusterings43.png) |

**Davies Bouldin Index for Feature Pair Clusterings:**
![GMM DBS](/music-clustering-report/assets/images/KM%20DBS.png)

**Silhouette Score for Feature Pair Clusterings:**
![GMM Silhouette](/music-clustering-report/assets/images/GMM%20Silhouette.png)

**Dunn Index for Feature Pair Clusterings:**
![GMM Dunn](/music-clustering-report/assets/images/GMM%20Dunn.png)

Next, we carried out the same procedure for string data. Using the elbow method with inertia to find the optimal number of clusters. We see that GMM was ill-suited for clustering string data.
![GMM Elbow Method String](/music-clustering-report/assets/images/GMM%20Elbow%20Method%20String.png)

Since we are using multiple internal measures, there lies inconsistencies on how well features are paired/clustered in comparison to others. For example, according to Silhouette Score, duration/tempo is the second best feature pair for clustering but if we look at the Davies Bouldin visualization, that one says duration/”end of fade in” is the second best clustered. In other words, the ranking of feature pairs differ depending on the metric.

To remedy this, we assigned a score/rank for each metric and then gave an average ranking across all metrics. For a given metric, the pair with the best clustering gets a score of 15. The pair with the second best clustering gets a 14 and so on. We then calculate the average across the scores to get the average rankings based on how well the pairs cluster.

**GMM Feature Pairwise Clustering Rankings**
![GMM Rankings Chart](/music-clustering-report/assets/images/GMM%20Ranking%20Chart.png)

| Feature Pair                             | Score |
| ---------------------------------------- | ----- |
| `artist_familiarity`/`end_of_fade_in`    | 1.0   |
| `duration`/`end_of_fade_in`              | 2.0   |
| `artist_familiarity`/`loudness`          | 3.0   |
| `end_of_fade_in`/`loudness`              | 4.0   |
| `end_of_fade_in`/`tempo`                 | 5.0   |
| `artist_familiarity`/`tempo`             | 6.0   |
| `end_of_fade_in`/`start_of_fade_out`     | 7.0   |
| `loudness`/`tempo`                       | 8.0   |
| `duration`/`loudness`                    | 9.0   |
| `artist_familiarity`/`start_of_fade_out` | 10.0  |
| `artist_familiarity`/`duration`          | 11.0  |
| `loudness`/`start_of_fade_out`           | 12.0  |
| `duration`/`tempo`                       | 13.0  |
| `start_of_fade_out`/`tempo`              | 14.0  |
| `duration`/`start_of_fade_out`           | 15.0  |

Looking at the results, we can see that songs are best clustered by artist name and release (album name), which makes intuitive sense. The best numerical feature pair is duration and start of fade out. The worst was the “end of fade in” and “artist familiarity”.

### General Discussion

To compare performance between methods, we mainly look at Silhouette scores and Davie Bouldin indices since these are the metrics shared between all three methods. We found that K-Means and GMM produced relatively similar results across the pairs of variables, and ranked them similarly. Duration and tempo seemed to be important variables to cluster on. For Hierarchical Clustering, we found that performance across different pairs of variables were more consistent and generally better than the other two methods. For example, HC had a Silhouette score ranging from around 0.3 to 0.5 across all pairs, whereas K-Means and GMM ranged from around -0.1 to 0.25 across all pairs (higher is better for Silhouette score). We also found that K-Mean and GMM had a much higher variance on the Davie Bouldin index, with values ranking from 0 to 35 while HC was consistently between 0 and 1 (lower is better for DB index). From this, we conclude that Hierarchical Clustering performed the best. This intuitively makes sense because songs typically fall into hierarchical genres and subgenres.

### Next Steps

Currently, we have implemented K-Means clustering, Gaussian Mixture Model Clustering, and Hierarchical Clustering. For our final report, we plan on implementing DB-SCAN (Density-Based Spatial Clustering of Applications with Noise) in addition to the three models we have right now, especially since DB-SCAN provides the unique opportunity to try out density-based clustering. Additionally, DB-SCAN is well known to be advantageous in handling noise, making it robust to outliers. This will be a significant deviation from the models we are using, as K-Means, GMM, and Hierarchical clustering all have limitations when it comes to blocking noise and preventing outliers from skewing data.

## References

[1] T. Bertin-Mahieux, D. P. W. Ellis, B. Whitman, and P. Lamere, "The Million Song Dataset," in Proceedings of the 12th International Society for Music Information Retrieval Conference (ISMIR 2011), 2011.

[2] Ahmet Elbir and Nizamettin Aydın, “Music genre classification and music recommendation by using deep learning,” Electronics Letters, vol. 56, no. 12, pp. 627–629, Jun. 2020, doi: https://doi.org/10.1049/el.2019.4202.

[3] Y.-H. Cheng and C.-N. Kuo, “Machine Learning for Music Genre Classification Using Visual Mel Spectrum,” Mathematics, vol. 10, no. 23, pp. 4427–4427, Nov. 2022, doi: https://doi.org/10.3390/math10234427.

[4] T. Li, Mitsunori Ogihara, W. Peng, B. Shao, and S. Zhu, “Music Clustering With Features From Different Information Sources,” IEEE Transactions on Multimedia, vol. 11, no. 3, pp. 477–485, Apr. 2009, doi: https://doi.org/10.1109/tmm.2009.2012942.

[5] P. Georges and Ngoc Cuong Nguyen, “Visualizing music similarity: clustering and mapping 500 classical music composers,” Scientometrics, vol. 120, no. 3, pp. 975–1003, Jul. 2019, doi: https://doi.org/10.1007/s11192-019-03166-0.

‌[6] subha5gemini, “MillionSongDataset/subset-compiled.csv,” Github, Apr. 09, 2023. https://github.com/subha5gemini/MillionSongDataset/blob/master/subset-compiled.csv
‌
‌

## Contributions

The following contribution table displays responsibilities that each member performed in completing this report:

| Member       | Contributions                                                                                          |
| ------------ | ------------------------------------------------------------------------------------------------------ |
| Ethan Torone | GMM - Coding & Results/Discussion                                                                      |
| Edward Chen  | HC - Coding & Results/Discussion                                                                       |
| Brian Pak    | Data Cleaning/Pre-Processing - Coding & Reporting, K-Means - Coding, Introduction & Problem Definition |
| John Pham    | K-Means - Coding & Results/Discussion                                                                  |
| Vibhav Kumar | Data Cleaning & Pre-Processing - Coding & Reporting, Next Steps                                        |
