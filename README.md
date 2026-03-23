# Herring-spatial-analysis
## I Background
- Baltic herring (Clupea harengus) is a key species in the Baltic Sea ecosystem, linking lower and higher trophic levels.
- Understanding its spatial distribution and potential subpopulation structure is essential for biodiversity conservation and fisheries management.
- This project applies data science and machine learning methods to explore herring occurrence patterns using open biodiversity data.
## II Methods
- Data cleaning and filtering
- Kernel Density Estimation (KDE) for hotspot detection
- DBSCAN for spatial clustering
- Temporal analysis (monthly patterns)
## III Results
### Occurence Map
![Map](figures/occurrence_map.png)
### Hotspot Map
![Hotspot](figures/hotspot_map.png)
### Clustering (eps = 0.3)
![Cluster](figures/clusters_eps_0.3.png)
## IV Data Source / Citation
 Data used in this project were obtained from the Global Biodiversity Information Facility (GBIF). Access via https://www.gbif.org
 Data were filtered for the Baltic Sea region (Sweden, Denmark, Poland)
