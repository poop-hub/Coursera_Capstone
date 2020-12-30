# The Battle of Houston Neighborhoods

## Introduction: Business Problem

In this project, we will try to find the best type of store for opening and its location. More specifically, it will be useful for stakeholders looking to open a store in Houston. Since the city is large, there are many venues in it. There are different neighborhoods in it in terms of living standards. Therefore, what exactly to open a store and where you want to find out. It is interesting to know where wealthy people live and which store can be opened as close as possible to them. We will use knowledge gained in specialization and data from open sources to try to solve this problem.

## Data

In accordance with the problem under consideration, we will be interested in the following factors:

- prevalence of certain types of shops in the city
- the location of these stores
- areas with wealthy residents
- type of store to open
- approximate store location

centers of candidate areas will be generated algorithmically and approximate addresses of centers of those areas will be obtained using Google Maps API reverse geocoding
number of venues and their type and location in every neighborhood will be obtained using Foursquare API
coordinate of Houston center will be obtained using Google Maps API geocoding of well known Houston Center

## Methodology

In this project, we will investigate Houston for the possibility of opening a store and identify an approximate area in which to do so. The city is located in Texas and has about 2 million inhabitants. In the first step, we will collect data to describe the city at the district level. We will also collect data on venuses located in this city in order to determine where the density of vents is higher and there is a demand and an opportunity to discover a new one. Next, we will only work with data for all stores to determine the density of stores in the city and find a niche. Next, we will find common patterns between areas in order to determine the approximate contingent of our clients. We will use two types of clustering to make sure that the clusters reflect common patterns. For this, we use the KMeans and Agglomerative Clustering algorithms. Finally, we will narrow down the category of stores that can be opened in one of the districts of these clusters. We will then use the data for the wealthiest neighborhoods in the city to find neighborhoods where customers can afford to shop in our store. As a result, we will get the approximate location of the store and the type of store that have development prospects.

## Analysis

### 1. Displaying city neighbourhoods

To begin with, we load ready geojson coordinates regions and their names. We define a function to get the coordinates of the neighbourhoods centers. We assign each area a unique number to paint it on the map in a specific color. Next, we identified 88 main areas of the city and drew a choropleth map. Colors are limited, so some areas have the same color. The black dot corresponds to the city center.

![ ](png/houston_map.png  "Map of Houston")

It can be seen that the city center is slightly shifted relative to the tinted area of the districts. The city has a large number of districts, so it is necessary to determine which establishments are located in them and where.

### 2. Loading nearby venues

Now we want to find out which venues are located in this city within a 2 km radius from the neighbourhoods centers. We use the Foursquare API for this. We set API request functions and get about 5000 venues with coordinates. We also get their categories. This is necessary to determine factorial variables in cluster analysis. We marked specific venues in red. The city center is represented by a black circle. We also marked the boundaries of the neighbourhoods.

![ ](png/houston_venue_map.png  "Map of Houston Venues")

It seems the streets flooded hundreds of zombies! It can be seen that the venues are unevenly distributed in the city. Most of them are concentrated in the center and southwest. It can be seen that some streets are very densely filled, while others are empty.

### 3. Analysis of prevalence stores

We want to open a store, so knowing where venues are located, we can analyze which stores are present in the city. This will identify the most popular types of stores. With this approach, we can narrow down the search for a suitable store.

![ ](png/shop_counts.png  "Most popular types of shops")

The most common shops in the city:

- Discount Store, 
- Coffee Shop, 
- Grocery Store, 
- Ice Cream Store, 
- Mobile Phone Store, 
- Donut Store

Indirectly, it can be argued that these stores are popular with city residents. Therefore, opening one of these stores can be successful. There are two options: expanding the existing network of such stores or creating a new one.

### 4. Identifying patterns in similar areas

To define patterns in the data, we will only consider a dataset with stores. We transform the dataset to determine the average occurrence of one type of store in each area relative to another type of store. Now we need to cluster the data. We will use two clustering algorithms for the top and bottom pass to be more sure that such clusters are really similar to each other. In this case, it is necessary that the number of clusters coincide. We will use KMeans and Agglomerative Clustering algorithms. For agglomerative clustering, 3 clusters are optimal, but KMeans has 6 clusters, which in the case of agglomerative clustering corresponds to the second local minimum. Next, we compare the two types of clustering for cluster similarity. Now we will have more similar clusters than those that do not match for different models. We add all cluster labels to the main dataset. Now we see that most of the labels are the same.

![ ](png/cluster_map.png  "Most similar clusters")

You can trust these two clusters to be clustered correctly with a high probability. Next, we will look at the distributions for each cluster. And we will determine what type of store can be opened in one of these areas. Now we can look at the similarities and differences in the three main clusters. For this we will use bar diagrams.
Find the prevalence of stores in the 5th cluster

![ ](png/shop_counts5.png  "Shop counts in cluster 5")

the prevalence of stores in the 1st cluster

![ ](png/shop_counts1.png  "Shop counts in cluster 1")

The prevalence of stores in the 3rd cluster

![ ](png/shop_counts3.png  "Shop counts in cluster 3")

We see that the stores in all clusters are exponentially distributed. The largest number of stores corresponds to large chains. In cluster 3, there is a large number of second-hand shops, which may indicate that poor residents live in such areas or come here from other areas. We see that in the 1st cluster mobile phone shops are widespread, in other clusters they are less widespread. We can see that discounters are common across all clusters. Coffee shops are just as common. Grocery stores are not common in cluster 3. Mobile phone stores are distributed in 1 cluster. It is also seen that clusters 1 and 5 are very similar to each other. There are approximately the same number of discounters, coffee shops and groceries. Thus, it can be assumed that the lack of mobile phone stores in cluster 5 may be an existing niche.

### 5. Determining the type and location for opening a store

We need to find suitable locations for our stores. Obviously, the location of the store will be determined by the lack of similar stores nearby, the possibility of opening a store in this place and the demand for the product. It is logical to assume that the demand will be associated with the solvent category of citizens. We'll download the latest data for 10 of Houston's wealthy middle-income neighborhoods. Since their names are slightly different from the names in the geojson file, we need to load their geodata. Let's use the already created function. Using the coordinates of these areas, we can determine the median center around which the safe areas are located. Then, using the nearest neighbors algorithm, we can determine the equidistant areas from this coordinate. So we will determine in which neighbourhoods there are solvent citizens.

![ ](png/top_map.png  "Most prosperous neighbourhoods")

We can see that the best neighbourhoods are located west of the city center, with their median center located at River Oaks. It will most likely be impossible to open a store in the very center of this area due to the rental cost or because of the closedness of the area. Therefore, we will find the closest areas to these. It is logical to assume that residents will tend to live closer to good areas, so they will occupy nearby areas of the city. You can open a shop in one of these places. Find the nearest neighborhoods using the nearest neighbors model.

We can now identify the most common stores in these areas.

![ ](png/shop_counts_top.png  "Top neighbourhood's shop counts")

It can be seen that mobile phone shops are not widespread in the western part of the city. Let's build a map of the area in which stores can be opened.

![ ](png/phone_map.png  "Mobile phone shops map")

As you can see, there are a large number of shops in the desired area. It follows from this that there is an opportunity to build stores there and it is profitable. However, mobile phone shops are located outside the desired area. Therefore mobile phone shop can be opened in one of the surrounding areas.

## Results and Discussion

Our analysis showed that there is a large number of establishments in Houston. A large number of stores are chain stores. The most common stores are Discount Shop, Coffee Shop, Grocery Shop. There are prosperous, high-income areas in the city. In the vicinity of these areas, you can build shops that are not found in these areas due to certain circumstances. We found out that one of the promising stores to open are mobile phone stores. Currently, most of the inhabitants of developed cities have a mobile phone and this trend is positive. In developed areas, such shops do not meet, which may not be very convenient for wealthy residents. Thus, opening a store in one of the areas close to prosperous is a promising business start. We found that such areas are located to the west of the city center and many of them are clustered in a similar way, which suggests that the consumer interests of citizens are approximately similar. That is, citizens from areas where there are no mobile phone stores may be inclined to shop in such stores as citizens in areas similar to this.

## Conclusion

The goal of this project was to identify the type of store and its approximate location in a promising area of the city of Houston for stakeholders. Using open sources of data, we created datasets that helped identify patterns in the data. Data clustering made it possible to identify similar areas by the contingent of buyers. As a result, we identified a possible store location to the west of the city center and decided that it would be optimal to open a mobile phone store there.
