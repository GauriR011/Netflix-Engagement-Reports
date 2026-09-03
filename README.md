# Netflix Movies: Performance Segmentation and Global Availability

## Overview
This project analyzes Netflix movie viewing data from Netflix’s Engagement Reports covering the period from July 2023 to June 2025. The dataset is publicly available through the TidyTuesday GitHub repository.

The analysis focuses exclusively on the Movies dataset and investigates whether Netflix movies can be grouped into distinct performance segments based on viewer engagement and viewing efficiency, and whether global availability is associated with these performance segments.

The analysis uses movie runtime, views, and global availability to understand differences in movie performance. Viewer engagement is examined through the relationship between a movie’s runtime and its number of views, while viewing efficiency measures views relative to the length of the movie.


## Dataset Info

The dataset contains `36,121 observations` and `8 variables`. It comes from Netflix Engagement Reports and includes information about movies, their availability, and their viewing performance.

Important variables used in this analysis include:
| Feature Name | Feature Description |
| --- | --- |
| `title` | Movie title |     
| `available_globally` | Whether the movie is available globally (Yes/No) |   
| `release_date` |  Movie release date |   
| `hours_viewed` | Total hours the movie was viewed |
| `runtime` | Movie duration, originally stored as a string in hours, minutes, and seconds |
| `views` | Number of views received by the movie |

For the clustering analysis, the following derived variables were created:
| Feature Name | Feature Description |
| --- | --- |
| `total_runtime_mins` | Total movie runtime in minutes |
| `log_views` |  Log-transformed number of views |
| `log_efficiency` | Log-transformed viewing efficiency, calculated as views divided by total runtime in minutes. This represents the number of views relative to the length of a movie |

Before clustering, observations with missing runtime or view values were removed. Movies with runtimes outside the selected range of 20 to 210 minutes were also excluded, and the resulting variables were standardized before clustering.


## Questions We Are Trying to Answer

The project focuses on two main questions:

`1. Do Netflix movies form distinct performance segments?`

Specifically, can movies be grouped into meaningful segments based on:
- Overall viewership/engagement
- Movie runtime
- Viewing efficiency, or views relative to movie length


`2. Does global availability affect movie performance?`

After creating the performance segments, we investigate whether movies that are globally available are more likely to belong to higher-performing segments than movies that are not globally available.


<!--
What Did We Do to Answer the Questions?
1. Data Cleaning and Preparation

We first removed observations with missing runtime or views.

Movie runtimes were converted from their original hours/minutes format into a single value representing total runtime in minutes.

We then created two additional measures:

A log-transformed view count to make the highly skewed viewership data easier to analyze.
A log-transformed efficiency metric representing views relative to movie runtime.

The resulting clustering variables were runtime, log views, and log efficiency.

2. Outlier Removal and Scaling

Extremely long or short movie runtimes were removed using a selected runtime range of 20–210 minutes. The resulting variables were then scaled so that differences in measurement units would not disproportionately influence the clustering process.

3. Selecting the Number of Clusters

We used the Elbow Method to determine an appropriate number of clusters for K-Means clustering.

The analysis tested cluster sizes from 2 through 15 and compared the total within-cluster sum of squares. Based on the resulting elbow analysis, four clusters were selected.

4. K-Means Clustering

K-Means clustering was then applied using four clusters.

The clusters were interpreted based on their runtime, number of views, and viewing efficiency. They were subsequently given descriptive names:

Cluster A — High-performing
Cluster B — Short-form Moderate Performers
Cluster C — Long-form Moderate Performers
Cluster D — Low-performing
5. Comparing Global Availability

Finally, the cluster assignments were combined with the original movie information.

For each cluster, we calculated the number and percentage of movies that were:

Globally available
Not globally available

A 100% stacked bar chart was then used to compare the proportions across clusters.
-->

## Visualizations Plotted

### 1. Elbow Plot
Purpose: Determine the appropriate number of clusters for K-Means.

The plot shows the total within-cluster sum of squares for different values of k. This was used to select `4 clusters` for the final analysis.

<p align="center">
  <img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/bc6ff73d-fbf8-4da5-a935-5776d0f7b76d" />
</p>

### 2. Runtime vs. Viewing Efficiency Scatter Plot

This scatter plot shows the relationship between movie runtime and viewing efficiency, with movies colored according to their cluster.

It helps visualize the separation and overlap between the four performance segments and shows how viewing efficiency changes as runtime increases.

<p align="center"> 
  <img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/7a21c638-811f-4404-b8b1-00dc28e65891" />
</p>

### 3. Viewing Efficiency by Cluster — Box Plot

This box plot compares the distribution of viewing efficiency across the four clusters. It allows us to compare the median efficiency and variability of each performance segment.

<p align="center">
  <img width="450" height="400" alt="image" src="https://github.com/user-attachments/assets/90f8da60-adaa-4e20-8051-94bb5e6744b8" />
</p>

### 4. Distribution of Views by Cluster — Sina + Violin + Box Plot

This compound visualization combines:

A `Sina plot` to show individual observations and their spread.
A `Violin plot` to show the distribution and density.
A `Box plot` to summarize the central tendency and variability.

Together, these plots allow us to compare viewership distributions across the four clusters.

<p align="center">
  <img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/b3c443e8-2c80-4ed6-8f9e-44545d63cb74" />
</p>

### 5. Viewership Distribution among Clusters based on Global Availability of Movies — 100% Stacked Bar Chart

This chart compares the percentage of globally available and non-globally available movies within each cluster.

A `100% stacked chart` was chosen because the goal was to compare proportions, rather than absolute cluster sizes.

<p align="center">
  <img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/881fb24c-203b-41c1-9d82-a8e55ba6c8de" />
</p>

## Results and Inference

Four Distinct Performance Segments Were Identified

The K-Means analysis identified four distinct Netflix movie performance segments:

#### Cluster	Segment	Characteristics
| Cluster | Segment | Characteristics |
|---------|---------|-----------------|
| **A** | High-performing | Highest engagement and viewing efficiency |
| **B** | Short-form Moderate Performers | Shorter runtime with moderate engagement and relatively strong efficiency |
| **C** | Long-form Moderate Performers | Longer runtime with moderate engagement and lower efficiency |
| **D** | Low-performing | Lowest engagement and viewing efficiency |

  - Cluster A had an average runtime of approximately 109 minutes
  - Cluster B approximately 94 minutes
  - Cluster C approximately 138 minutes
  - Cluster D approximately 101 minutes.

#### `Shorter Movies Tend to Perform Better Among Moderate Performers`

A notable difference was observed between Clusters B and C.

Cluster B, consisting of shorter movies, achieved approximately 700K views, while Cluster C, consisting of longer movies, achieved around 300K views.

This suggests that, among moderately performing movies, shorter runtimes are generally associated with higher engagement.

#### `Runtime Matters, but It Does Not Explain Everything`

The analysis suggests that viewing efficiency generally decreases as runtime increases.

However, runtime alone does not determine whether a movie performs well.

For example, Cluster D has an average runtime similar to the higher-performing Clusters A and B, but has substantially lower engagement and viewing efficiency. This indicates that other factors besides runtime contribute to movie performance.

Interestingly, Cluster A manages to maintain high efficiency across different movie lengths, whereas the other clusters show a stronger decline in efficiency as runtime increases.

#### `Most Movies Fall Into the Low-Performing Segment`

Cluster D is by far the largest segment, containing approximately 16,500 movies, or about 46% of the dataset.

In comparison, Cluster A contains approximately 6,000 movies, representing about 17% of the dataset.

This suggests that the highest-performing segment represents a relatively small portion of the Netflix movie catalog, while a much larger share of movies falls into the low-performing category.

#### `Global Availability Is Associated With Higher Performance`

There is also a clear relationship between global availability and cluster performance.

The percentage of globally available movies in each cluster was:

| Cluster A: 32.5%  |  Cluster B: 26.5%  |  Cluster C: 18.2%  |  Cluster D: 10.2%    |
| --- | --- | --- | --- |

The ordering of performance is:

`**Cluster A > Cluster B > Cluster C > Cluster D**`

The same ordering is observed in the percentage of movies that are globally available.

Cluster A movies are therefore nearly three times as likely to be globally available as Cluster D movies.

Overall, the analysis suggests a positive relationship between global availability and movie performance. Movies accessible to a wider international audience appear more likely to fall into the higher-performing clusters.


## Conclusion

The analysis shows that Netflix movies can be separated into four meaningful performance segments based on runtime, engagement volume, and viewing efficiency.

The results indicate that:

1. High-performing movies combine strong viewership with high viewing efficiency.
2. Shorter movies generally perform better than longer movies among the moderate-performing segments.
3. Viewing efficiency tends to decline as movie runtime increases, although high-performing movies can maintain strong efficiency across different runtimes.
4. Runtime alone does not determine movie performance, as movies with similar runtimes can belong to very different performance segments.
5. Global availability is positively associated with performance: the highest-performing cluster has the greatest proportion of globally available movies, while the lowest-performing cluster has the smallest proportion.

Therefore, the analysis provides evidence that both movie runtime and global availability are important factors associated with Netflix movie performance, while also showing that runtime by itself cannot fully explain differences in engagement.
