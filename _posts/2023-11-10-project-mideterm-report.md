---
layout: post
title: "Project Midterm Report"
---

## Introduction & Background

Music streaming platforms like Spotify and SoundCloud have greatly increased the consumption of music throughout recent years. To keep users engaged with their products, these services have invested heavily into music recommendation algorithms, spurring the research domain of Music Information Retrieval (MIR).

Within MIR, researchers have investigated two central problems in music recommendations: the classification and clustering of music data. Many studies have already presented supervised methods like CNNs [2, 3] to assign music tracks to a collection of labeled genres. Other common genre classifiers include Support Vector Machines (SVMs) and K-Nearest Neighbor classifiers (KNNs). However, music genres are largely subjective, and they vary depending on the listener's musical background, especially with names like “Classical Rock” and “Hardcore Punk”. Researchers have instead pursued unsupervised methods to recommend songs with similar audio qualities without user labeling. For example, Georges and Nguyen [5] use K-Means and Hierarchical clustering to group works from over 500 European classical composers. Other researchers like Li et al. [4] propose new clustering methods like bimodal clustering to assist with Music IR tasks.

In this project, we explore how clustering analysis can be applied to the [Million Song Dataset](http://millionsongdataset.com/) [1], a collection of audio-features and metadata for over 1 million contemporary music tracks collected by the Echo Nest. This dataset consists (i.e. loudness, start of fade out, end of fade in, duration), musicality (time signature, beat number, key, tempo), and popularity (i.e. song and artist popularity). Due to computational limitations, however, we will use a subset of 10,000 songs from this dataset [6].

## Problem Definition

With the advent of digital music platforms and streaming services, there is an overwhelming abundance of music available for streaming. Users often struggle to navigate through extensive catalogs and playlists to find music that best aligns with their current music preferences and moods. As mentioned previously in our introduction, we cannot rely on genre to suggest similar songs due to its subjective nature. Therefore, we will be applying clustering algorithms like K-Means, GMM, & Hierarchical Clustering to see if there are any attributes other than genre that help relate songs. If such attributes exist, we would also want to identify which features we can use to assist listeners finding similar music tracks.

## Methods

### Data-Cleaning

We began our data-cleaning pipeline by cleaning any of our text attributes from our initial dataset. We noticed that the string values for attributes like release and artist name we wrapped by quotations and prepended by the letter “b”. We removed these characters along with stop words like “A” and “I” that do not provide much significance. We also inspected the number of missing values in our data, and found that only four points were missing a value for artist familiarity. We replaced these values with the median value for artist familiarity.

In one of the final steps of our data cleaning process, we inspected the confidence values for all data points. The Million Song Dataset includes confidence measures for attributes like time signature. Due to the nature of these attributes, it’s possible that some of the data points have confidence values close to zero, suggesting that the corresponding values were estimated rather than accurately measured. We removed any data points with a confidence value less than a threshold of 0.2 for time signature, mode, and key. We also removed any songs with no duration, tempo, or time signature.

### Pre-processing

The Echo Nest, the organization responsible for maintaining the Million Song Dataset, includes audio features like segments, tantums, and bars. These attributes contain lengths for the units of audio analysis that follow a modified version of the MIDI time format. For the purpose of producing results in a timely manner, we removed these attributes from our dataset. We also remove attributes like “danceability” and “energy” which have not been analyzed by the Echo Nest.

### Unsupervised Methods

As of the time of writing this report, we have applied the following clustering algorithms to our dataset using the scikit-learn library:

- K-Means Clustering
- GMM
- Hierarchical Clustering

We plan on implementing DB-SCAN to include in our final report. The following presents the results that we found.

## Results & Discussion

### K-Means

### Hierarchical Clustering

### Gaussian Mixture (GMM)

## References

[1] T. Bertin-Mahieux, D. P. W. Ellis, B. Whitman, and P. Lamere, "The Million Song Dataset," in Proceedings of the 12th International Society for Music Information Retrieval Conference (ISMIR 2011), 2011.

[2] Ahmet Elbir and Nizamettin Aydın, “Music genre classification and music recommendation by using deep learning,” Electronics Letters, vol. 56, no. 12, pp. 627–629, Jun. 2020, doi: https://doi.org/10.1049/el.2019.4202.

[3] Y.-H. Cheng and C.-N. Kuo, “Machine Learning for Music Genre Classification Using Visual Mel Spectrum,” Mathematics, vol. 10, no. 23, pp. 4427–4427, Nov. 2022, doi: https://doi.org/10.3390/math10234427.

[4] T. Li, Mitsunori Ogihara, W. Peng, B. Shao, and S. Zhu, “Music Clustering With Features From Different Information Sources,” IEEE Transactions on Multimedia, vol. 11, no. 3, pp. 477–485, Apr. 2009, doi: https://doi.org/10.1109/tmm.2009.2012942.

[5] P. Georges and Ngoc Cuong Nguyen, “Visualizing music similarity: clustering and mapping 500 classical music composers,” Scientometrics, vol. 120, no. 3, pp. 975–1003, Jul. 2019, doi: https://doi.org/10.1007/s11192-019-03166-0.

‌[6] subha5gemini, “MillionSongDataset/subset-compiled.csv,” Github, Apr. 09, 2023. https://github.com/subha5gemini/MillionSongDataset/blob/master/subset-compiled.csv
‌
‌
‌

## Contributions

The following contribution table displays responsibilities that each member performed in completing this report:

| Member       | Contributions                                                                  |
| ------------ | ------------------------------------------------------------------------------ |
| Ethan Torone | GMM - Coding & Results/Discussion                                              |
| Edward Chen  | HC - Coding & Results/Discussion                                               |
| Brian Pak    | Data Cleaning/Pre-Processing, K-Means Coding, Introduction, Problem Definition |
| John Pham    | K-Means Coding & Results/Discussion                                            |
| Vibhav Kumar | Data Cleaning & Pre-Processing                                                 |
