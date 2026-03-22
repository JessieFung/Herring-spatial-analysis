# Herring-spatial-analysis
# 🌍 Background

# Baltic herring (Clupea harengus) is a key species in the Baltic Sea ecosystem, linking lower and higher trophic levels. 

# Understanding its spatial distribution and potential subpopulation structure is essential for biodiversity conservation and fisheries management.

# This project applies data science and machine learning methods to explore herring occurrence patterns using open biodiversity data.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.neighbors import NearestNeighbors
import numpy as np
df = pd.read_csv("D:/Study in SU/Herring study/Herring from GBIF .csv",sep="\t")
df.head()
df.columns

df= df[[
    'decimalLatitude','decimalLongitude','eventDate','countryCode'
]]

df = df.dropna(subset=['decimalLatitude','decimalLongitude'])

df['eventDate'] = pd.to_datetime(df['eventDate'], errors='coerce')

df = df.dropna(subset=['eventDate'])

df = df[
    (df['decimalLatitude'] > 53) &
    (df['decimalLatitude'] < 66) &
    (df['decimalLongitude'] > 10) &
    (df['decimalLongitude'] < 30)
]
df['month'] = df['eventDate'].dt.month
df['year'] = df['eventDate'].dt.year

plt.figure(figsize=(6,4))
plt.scatter(df['decimalLongitude'], df['decimalLatitude'], s=1)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Herring Occurence Map')
plt.show()

plt.figure(figsize=(6,4))
sns.kdeplot(
    x=df['decimalLongitude'],
    y=df['decimalLatitude'],
    fill=True
)
plt.title('Herring Hotspot Map')
plt.show()

sns.histplot(df['month'], bins=12)
plt.title('Monthly Distribution of Herring')

from sklearn.cluster import DBSCAN

coords = df[['decimalLatitude', 'decimalLongitude']]
neighbors = NearestNeighbors(n_neighbors=20)
neighbors_fit = neighbors.fit(coords)
distances, indices = neighbors_fit.kneighbors(coords)

distances = np.sort(distances[:,19])

plt.plot(distances)
plt.yscale('log')
plt.title('k-distance graph')
plt.xlabel('Points sorted')
plt.ylabel('LogDistance')
plt.show()

for eps in [0.2,0.25,0.3]:
    model = DBSCAN(eps=eps, min_samples= 20)
    df['cluster'] = model.fit_predict(coords)
    
    plt.figure(figsize=(8,6))
    df_main = df[df['cluster'] != -1]
    plt.scatter(
    df_main['decimalLongitude'],
    df_main['decimalLatitude'],
    c=df_main['cluster'],
    s=5,
    cmap='tab10'
)
    df_noise = df[df['cluster'] == -1]
    plt.scatter(
    df_noise['decimalLongitude'],
    df_noise['decimalLatitude'],
    color='black',
    s=5,
    label = 'Noise(-1)'
)
    plt.xlabel('Longitude')
    plt.ylabel('Latitude')
    plt.title(f'Herring Clusters (DBSCAN, eps={eps})')
    plt.legend()
    plt.show()
    print(f'\n === eps={eps}===')
    print(df['cluster'].value_counts())
    plt.figure(figsize=(8,6))
    sns.histplot(
    data=df,
    x='month',
    hue='cluster',
    bins=12,
    multiple='dodge',
    palette='tab10'
)
    plt.title(f'Monthly Distribution by Cluster (eps = {eps})')
    plt.xlabel('Month')
    plt.show()