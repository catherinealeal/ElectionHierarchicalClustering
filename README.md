# Hierarchical Clustering of Election Data and Cluster Evaluvation 

## Goal 

Use hierarchical clustering to group states based on votes cast in past elections. The groups generated could be used to inform political campaigns and identify swing states. 

## Data 

The data used is from the American Presidency Project and is split into two files:
- votes_by_state.csv has the raw number of ballots cast for each of the top presidential candidates from 2008-2012 by US State (and the District of Columbia) - [Dataset](https://gist.githubusercontent.com/TieJean/a63b4d51246aec6b73bb75944dd69ead/raw/292d338340efa19b8b47958ee4cfcca5e3d8e09c/votes_by_state.csv)
- republican_percentage_by_state.csv has the percentage of votes that were for the Republican candidate for 2008, 2012, and 2016 by US State (and the District of Columbia) - [Dataset](https://gist.githubusercontent.com/TieJean/74d13de3875643140d620f0664e1c933/raw/294658528271118725d4b10779cbb430597cf07d/republican_percentage_by_state.csv)

## Testing different proximity measures

Plot the hierarchical clusters formed from the raw vote count using 3 proximity measures: ward, min, and max. 

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/5125b397-4f53-444a-a190-a18d1385288e)

Figure 1. Hierarchical clustering of raw ballot data with Ward proximity measure. 

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/d6edb120-a2b1-4fe2-a74c-2579515241a7)

Figure 2. Hierarchical clustering of raw ballot data with Min proximity measure. 

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/1b65927d-eafc-4e1a-8cd1-ada3ba11b258)

Figure 3. Hierarchical clustering of raw ballot data with Max proximity measure. 

With each proximity measure, Montana, Wyoming, and Alaska all tend to cluster together. More suprisingly, New York is consistently clustered closer to Texas than it is to California. It seems that the population of each state is more important than which party actually won that state. With this in mind, it would be better to use percentages of votes. The second dataframe, republican_percentage_by_state.csv, was engineered from 7 attributes to 3 attributes where each column is the percentage of votes that were cast to the Republican candidate, and a 4th attribute indicating the range of percentages across the 3 elections.

## Re-test proximity measures on percentage data

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/4b9fb487-1bfc-4e8f-a2fb-d9f6e7080046)

Figure 4. Hierarchical clustering of percentage data with Ward proximity measure. 

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/30d1e300-fe8d-422e-abad-84aba42ed512)

Figure 5. Hierarchical clustering of percentage data with Min proximity measure. 

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/f1a730d2-5576-4211-8f94-1aa858f0d864)

Figure 6. Hierarchical clustering of percentage data with Max proximity measure. 

These hierarchies make more intuitive sense as New York and California are clustered closer than New York and Texas. 

## Calculate the Cophenetic Correlation Coefficient (CPCC)

Using the percentage data, calculate the CPCC for each of the three methods.

The CPCC evaluates the similarity between the pair-wise distance matrix of the original data and the clustering shown in the dendrogram. A high CPCC (close to 1) indicates that the dendogram well represents the original distances between points and therefore, that the clustering is accurate. 

 - CPCC with Ward proximity measure: 0.5883071696030502
 - CPCC with Min proximity measure: 0.695463585768092
 - CPCC with Max proximity measure: 0.7406937703705782

Based on the CPCCs just calculated, it seems that the max proximity measure provides the most accurate clustering of the data. 

## Find new clusters with k-means 

We now want to generate 4 clusters to inform 4 campaign strategies. Using the Ward proximity measure, implement the k-means algorithm using k=4 and the following initiation points: 
  - Montana
  - Arkansas
  - Massachusetts
  - Minnesota

Using random_state = 23. 

Clusters generated: 
- Cluster 0: Alabama, Arkansas, Idaho, Kentucky, Nebraska, North Dakota, Oklahoma, Tennessee, Utah, West Virginia, Wyoming
- Cluster 1: California, Dist. of Col., Hawaii, Maryland, Massachusetts, New York, Rhode Island, Vermont
- Cluster 2: Colorado, Connecticut, Delaware, Florida, Illinois, Iowa, Maine, Michigan, Minnesota, Nevada, New Hampshire, New Jersey, New, Mexico, Ohio, Oregon, Pennsylvania, Virginia, Washington, Wisconsin
- Cluster 3: Alaska, Arizona, Georgia, Indiana, Kansas, Louisiana, Mississippi, Missouri, Montana, North Carolina, South Carolina, South Dakota, Texas

## Visualize the Silhouette Coefficients for each cluster

Calculate the silhouette coefficient for each of the States in our data frame using the clustering from above.

![image](https://github.com/catherinealeal/ElectionHierarchicalClustering/assets/100166102/525c1eb1-59bd-4bce-b090-6055fc6ace26)

Figure 7. Silhouette coefficents of each cluster. 

- This plot shows us how well the clustering algorithm performed on this dataset. A high coefficient value (close to 1) indicates that the point fits well into its cluster, while a low coefficient indicates that that instance does not fit well into its cluster. 
- Based on just the shape/length of the bars, I would assume that the orange and blue clusters are less similar than the green and red clusters. This is because they are a lot less long and wide, meaning there are less points in these clusters and their silhouette coefficients are smaller. 
- Both the orange and blue clusters have points with negative coefficients, indicating that they are not very similar to other points in their cluster. The two points with negative coefficients are the states Nebraska and Rhode Island.

## View full notebook [here](https://github.com/catherinealeal/ElectionHierarchicalClustering/blob/5529779f483040cefec2f4f09b5414bcce07090e/ElectionHierarchicalClustering.ipynb)
