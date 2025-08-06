---
title: Geospatial Data Science 🗺
subject:  Story Map
subtitle: A case study in modern geodatabase
short_title: Geospatial Data Science
authors:
  - name: Mohamad Yassin
    affiliations:
      - AIRC
    email: mohamadyassin.us@gmail.com
license: CC-BY-4.0
keywords: geostpatial, data science, database, gis
abstract: |
  This is a story map about a school case study for the modern use of geospatial database. This article presents a concenptual solution for creating geospatial apps for a theme park.
kernelspec:
  name: python3
  display_name: Python 3
exports:
  - format: docx
  - format: pdf
    template: volcanica
    article_type: Report
---

# Geospatial Data Science 🗺

## Background

### How did we get here?
Geospatial isn’t only about geographic sciences. But also, about data and information technologies. Especially ever since the introduction of the global navigation satellite system (GNSS). However, both the geospatial and informational were either limited or separated for a long time. GPS technologies accelerated quickly over the past few decades. But it wasn’t an easy pursuit. It dates back to when, in order to satisfy their argument with the English about the shape of the Earth, the French sent out two expeditions to make spatial measurements. 

The French Cassini’s postulated that the Earth was elongated. While Isaac Newton deduced that it was flattened. In 1735 La Condamine and Bouguer went to what was then called Peru -in modern day Ecuador- to perform a measurement on a piece of a meridian. They spent nine years there. Then, in 1736, De Maupertuis spent a year and half in what which was then called Lapland in modern day Finland to do a similar land survey measurement. 

Contrary to what was originally anticipated. The French survey measurements affirmed that the Earth was indeed flattened -as Newton originally anticipated with mathematic and without ever leaving his desk. Those were the early expeditions that initiated Earth measurement sciences as we have them today. 

In 1807 President Thomas Jefferson, who was a surveyor himself, created the Survey of the Coast. Which included geodetic and astronomical survey tools to measure the Earth. Most of these tools were imported from Europe until the 1890s. The Coast Survey conducted monumental work across the U.S. collecting millions of data points on the ground in order to build complex mathematical models of the Earth. This later became the mapping system for the United States. Much of the work that these brave surveyors did over the past few decades laid out the groundwork for the evolution of GPS. In 1960 the U.S. Cost Survey figured out a way to use the Echo 1 satellite for global measurements of the earth. This initiated the development of the global satellite navigation network between 1964-1973. In 1970 the Coast Survey became the National Geodetic Survey (NGS) under NOAA. 

The high costs associated with satellites and mapping systems limited their access to government and military for a long time. But technological and industrial revolutions that have flattened the way over the past few decades lowered their barriers to entry. Anyone today can make geospatial measurements and conduct complex statistical and mathematical methods of the entire world without leaving his or her desk. Just like I'm about to demonstrate for my case study.

I copied all of the above information about the history of geodesy and surveying from video lectures by Dave Doyle, NGS, Chief Geodetic Surveyor (Retired). I included all the links to his awesome video lectures at the end if this page you're interested to learn a ton more.  


### Geodatabase Project
The objective of my assignment was to build a geodatabase and provide a useful application/service. I decided to find an open area near where I live to conduct a survey. So, I went to SeaWorld San Diego. What a great place to be. I created a geodatabase and visualized a map that can help both visitors and park management.

I was able to collect seven feature layers. Then I used an imagery web map from Esri with a WGS84 Web Mercator datum and PCS for my base map. Especially since the my map is dependant on GPS. For the geodatabase creation and web maps I used ArcGIS Pro and ArcGIS Online. 

In my case study, I demonstrated the following: concepts of the spatial and temporal aspects of GIS; cartography and symbology; geodatabase design and creation; network data; data collection; and data science and AI applications.

 

> Please note that all of the data included in my project was either publicly collected or fictional for the purposes of demonstration only.

## Geodatabase

### Base Map

I used an imagery basemap from Esri with WGS84 web mercator for the coordinate and projection systems. Image resolution is 0.3 meters. 

Beyond the base map, working with feature layers is similar to Object Oriented Programming (OOP). Each feature is a class with its own attributes and methods. The attributes are what goes into the dataset for each feature. I’m not going into any methods in my case study because I’m focusing on building the geodatabase. 

So, I defined the basic attributes for each of the following seven feature classes. I also used subtypes and domains examples to classify the data and restrict data entry errors. I included the metadata for a few attributes for each class in the side panel below. I also included domains and subtypes when available. Please scroll down to see them.

Please note that all of the data and attributes were fictional. None of the information is accurate or operational. It is intended only for demonstration purposes.

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 0.1, 'fillOpacity': 0.1},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;
m
```

### Walkways Network

It took me about 2 hours to walk the whole park and collect longitude and latitude coordinates for my feature classes. I also used Field Maps from Esri briefly to help draw features in the field. 

Furthermore, I spent a few hours on my base map to manually re-draw a walkway network and nodes. Neither horizontal nor vertical accuracy were important for my case study. I probably was able to attain 12-17 feet in general. 

For my walkways network, I ended up with 72 individual walkways -which were designed to look like the maze of an amusement park. 

In ArcGIS you can configure your network dataset properties for cost functions based on length, time or other attributes. You can also add rules, constraints, and directions, amongst other functions for network traffic  control. 

Network datasets usually have many attributes to define travel rules. My walkways feature class only included the geometry  with no further attributes for this case study. Further attribute definitions and properties configurations can be performed at a later time if we desire to create applications. 

I completed a  Network Analyst tutorial in ArcGIS Pro  before creating my own network dataset. In the tutorial you can look at the data attributes which can help decide which ones to choose when creating your own road or walkway network based on a predefined objective. Or you can look at smart city networks for a perspective about network datasets and systems in general.  

#### Metadata 
Field Name, Data Type, Numeric Format, Domain, Description


- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Polyline geometry zm

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

walkways = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
walkways = gpd.read_file(walkways)
if walkways.crs != "EPSG:3857":
    walkways = walkways.to_crs(epsg=3857)

m = walkways.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;
m
```

### Nodes
For my walkway network to perform properly if I wanted to create a direction app, I'd need to setup my roads based on the number of junctions in the network. 

There's a tool in ArcGIS Pro that will create the network dataset for you.  It includes a separate junction dataset by default. The junctions help with analyzing directions or finding best routes in a network. I created this nodes feature class manually just for visual purposes for now. 

I ended up with 55 junctions and dead ends. So, in short: my network has 72 walkways and 47 turns. Inside the area of approximately a square quarter mile. 


#### Metadata
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;

nodes = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Nodes.geojson"
nodes = gpd.read_file(nodes)
if nodes.crs != "EPSG:3857":
    nodes = nodes.to_crs(epsg=3857)

nodes.explore(m=m,
    marker_kwds={
        "radius": 6,           # Adjust the radius as needed
        "fill": True,          # Enable filling
        "fillColor": "yellow", # Set fill color to bright yellow
        "color": "red",      # Set outline color to red
        "weight": 2,          # Adjust outline width as needed
    },
    name="Your Points Layer", # Name for the layer controltiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;
m
```
### Points Of Interest (PoI) 
One of the first things visitors need to know in order to orient themselves are PoI. 

The PoI is a dataset that includes four feature subtypes. 

Vehicle traffic locations. Which include driving through the tolls into the large parking lots and the disabled and VIP parking areas symbolized near the park entrance. 
Points that indicate customer service interactions: Reservations, Sea Pixels, and Guest Services. 
Landmark points: park entrance and the Skyride Tower.
The two smoking areas. 
Subtype classification was used for symbology purposes in my case study. But this can also be used for operationalizing your data. 

Each of the four different subtypes  I defined can include a unique temporal dataset associated with it. For example, the vehicle traffic feature can include daily vehicle log information attached to them (see my survey example in the next section). 


#### Metadata
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point

- Name, Text, NA, NA, length 55

- Description, Text, NA, NA, length 500

- ID, Short, Numeric, NA, NA, Rank/Index

- *Type, Short, Numeric, NA, 0 Landmarks 1 Vehicle Traffic 2 Customer Service 3 Smoking Area

*Subtype

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;



nodes = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Nodes.geojson"
nodes = gpd.read_file(nodes)
if nodes.crs != "EPSG:3857":
    nodes = nodes.to_crs(epsg=3857)

nodes.explore(m=m,
    marker_kwds={
        "radius": 6,           # Adjust the radius as needed
        "fill": True,          # Enable filling
        "fillColor": "yellow", # Set fill color to bright yellow
        "color": "red",      # Set outline color to red
        "weight": 2,          # Adjust outline width as needed
    },
    name="Your Points Layer", # Name for the layer controltiles="Esri.WorldImagery",
    zoom_start=17  
)

poi = "https://github.com/mohamadyassin/myst_airc/releases/download/data/PointsofInterest.geojson"
poi = gpd.read_file(poi)
if poi.crs != "EPSG:3857":
    poi = poi.to_crs(epsg=3857)

poi_0 = poi[poi['Name'].isin(['Security', 'Sky Ride Tower'])]
poi_1 = poi[poi['Name'].isin(['Smoking Area'])]
poi_2 = poi[poi['Name'].isin(['Disables Parking', 'VIP Parking', 'Park Entrance'])]
poi_3 = poi[poi['Name'].isin(['Guest Services / Lost & Found', 'Sea Pixles', 'Reservations'])]

poi_0.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-circle-exclamation" style="color:red; background-color: white;  font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_1.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-smoking" style="color:white; background-color:black; border: 2px solid hotpink; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_2.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-car-side" style="color:blue; background-color:white; border: 2px solid blue; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_3.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-question" style="color: white; background-color:purple; border: 2px solid green; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

m
```

### Stadiums and Encounters (Experiences)
Stadiums and encounters are at the core of SeaWorld San Diego. They include entertainment and education experiences for all ages. You can see on the map how they are strategically distributed across the entire park. 

Experiences polygons were created for visual aid. But we can also look at them from a spatio-temporal perspective. This is one of the smartest and most widely used applications for GIS.  

The data can take a temporal aspect to measure the change. Instead of a passive point or area in space. We can measure the change in the non-spatial attributes. We can embed sensors, cameras, or any other method to collect data which also has spatial coordinates.

For instance, the zoological department can install sensors to or manually enter data into the geodatabase to monitor animals, and to measure temperatures, or health and safety indicators for the welfare of their animals. 

These attributes can be updated as frequently as desired. This is similar to what any web app database con do. But the spatial (geometry) attributes can help us further analyze or relate spatial phenomenon or analysis to our data. 


#### Metadata
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point geometry

- Name, Text, NA, NA, Length 25

- Type, Text, NA, NA, Length 25

- Description, Text, NA, NA, Length 400
- Duration, Text, NA, NA, Length 25

- WaitTime, Text, NA, NA, Length 25

- VIP, Text, NA, NA, VIPDom, Length 3

- *TypeCode, Short, Numeric, TypeDom, Three codes 1 Presentation 2 Event 3 Encounter

*Subtype

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi = "https://github.com/mohamadyassin/myst_airc/releases/download/data/PointsofInterest.geojson"
poi = gpd.read_file(poi)
if poi.crs != "EPSG:3857":
    poi = poi.to_crs(epsg=3857)

poi_0 = poi[poi['Name'].isin(['Security', 'Sky Ride Tower'])]
poi_1 = poi[poi['Name'].isin(['Smoking Area'])]
poi_2 = poi[poi['Name'].isin(['Disables Parking', 'VIP Parking', 'Park Entrance'])]
poi_3 = poi[poi['Name'].isin(['Guest Services / Lost & Found', 'Sea Pixles', 'Reservations'])]

poi_0.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-circle-exclamation" style="color:red; background-color: white;  font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_1.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-smoking" style="color:white; background-color:blue; border: 2px solid red; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_2.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-car-side" style="color:blue; background-color:white; border: 2px solid blue; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_3.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-question" style="color: yellow; background-color:green; border: 3px solid purple; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

events = "https://github.com/mohamadyassin/myst_airc/releases/download/data/StadiumsandEncounters.geojson"
events = gpd.read_file(events)
if events.crs != "EPSG:3857":
    events = events.to_crs(epsg=3857)

events.explore(m=m,
    style_kwds={"color": "pink", "weight": 3, "fill_opacity": 0.3},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

nodes = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Nodes.geojson"
nodes = gpd.read_file(nodes)
if nodes.crs != "EPSG:3857":
    nodes = nodes.to_crs(epsg=3857)

nodes.explore(m=m,
    marker_kwds={
        "radius": 6,           # Adjust the radius as needed
        "fill": True,          # Enable filling
        "fillColor": "yellow", # Set fill color to bright yellow
        "color": "red",      # Set outline color to red
        "weight": 2,          # Adjust outline width as needed
    },
    name="Your Points Layer", # Name for the layer controltiles="Esri.WorldImagery",
    zoom_start=17  
)


m
```

### Bathrooms
One of the most frequent questions asked by visitors: "where's the bathroom?" 

I counted 11 bathrooms but I believe that there are more. I bet that you can find a one easily within a short distance when visiting the park.

There aren't a whole lot of attributes for this feature class in my database. But, this depends on the objective of you feature class.

For my case it was just for symbology and wayfinding. But it can also be operationalized with a temporal aspect. For example, we can add date and cleaning attributes to be updated 4 time per day for each location.  We can also plug cleaning supplies information into our attributes.


#### Metadata 
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point geometry

- ID, Short, Numeric, NA, NA, Rank ID proximity based

- Type, Text, NA, NA, Default bathroom


```{code}
:tags: ["hide-input"]
import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi = "https://github.com/mohamadyassin/myst_airc/releases/download/data/PointsofInterest.geojson"
poi = gpd.read_file(poi)
if poi.crs != "EPSG:3857":
    poi = poi.to_crs(epsg=3857)

poi_0 = poi[poi['Name'].isin(['Security', 'Sky Ride Tower'])]
poi_1 = poi[poi['Name'].isin(['Smoking Area'])]
poi_2 = poi[poi['Name'].isin(['Disables Parking', 'VIP Parking', 'Park Entrance'])]
poi_3 = poi[poi['Name'].isin(['Guest Services / Lost & Found', 'Sea Pixles', 'Reservations'])]

poi_0.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-circle-exclamation" style="color:red; background-color: white;  font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_1.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-smoking" style="color:white; background-color:blue; border: 2px solid red; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_2.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-car-side" style="color:blue; background-color:white; border: 2px solid blue; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

poi_3.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-question" style="color: yellow; background-color:green; border: 3px solid purple; font-size:20px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

events = "https://github.com/mohamadyassin/myst_airc/releases/download/data/StadiumsandEncounters.geojson"
events = gpd.read_file(events)
if events.crs != "EPSG:3857":
    events = events.to_crs(epsg=3857)

events.explore(m=m,
    style_kwds={"color": "pink", "weight": 3, "fill_opacity": 0.3},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

restroom = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Restrooms.geojson"
restroom = gpd.read_file(restroom)
if restroom.crs != "EPSG:3857":
    restroom = restroom.to_crs(epsg=3857)

restroom.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-restroom" style="color: black; background-color:blue; border: 3px solid blue; font-size:12px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

nodes = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Nodes.geojson"
nodes = gpd.read_file(nodes)
if nodes.crs != "EPSG:3857":
    nodes = nodes.to_crs(epsg=3857)

nodes.explore(m=m,
    marker_kwds={
        "radius": 6,           # Adjust the radius as needed
        "fill": True,          # Enable filling
        "fillColor": "yellow", # Set fill color to bright yellow
        "color": "red",      # Set outline color to red
        "weight": 2,          # Adjust outline width as needed
    },
    name="Your Points Layer", # Name for the layer controltiles="Esri.WorldImagery",
    zoom_start=17  
)


m
```

### Rides
The rides had an interesting design. They were distributed along horizontal line bound SE-NW. 

I counted 11 rides. And I was able to create a template for a static display of ride information and attributes. 

The same information can also be dynamic in a web app. And the attributes can be edited and updated as necessary. 

The attributes were arbitrarily created to help demonstrate how to create a classification system in a web map to aid visitors in making decisions for which rides they'd want to visit first.


#### Metadata
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, Numeric, NA, Global ID

- Shape, Geometric, NA, NA, Point Geometry z m

- ID, Short, Numeric, NA, NA, Rank ID proximity based

- Name, Text, NA, NA, Ride name length 255

- Description, Text, NA, NA, Length 450

- AgeGroup, Text, NA, AgeDom, Three groups: Kids Adults/Kids Adults -Adults=12 yo or above

- AgeGroupType, Short, Numeric, NA, Three types 1 2 3

- AverageTime, Text, NA, NA, Length 25

- AgeTimeType, Short, Numeric, NA, Three types 1 2 3

```{code}
:tags: ["hide-input"]


import leafmap.foliumap as leafmap

m = leafmap.Map(center=[40, -100], zoom=4)
m
```
### Food & Beverage (F&B)
Walking the whole park isn't going to be easy without stopping for food. SeaWorld has a lot of options around nearly every junction. 

The F&B feature class was created for visualizing and wayfinding for this case study. However, I created some basic attributes to help visitors narrow down their F&B choices based on price and vegetarian food options for example. This feature class can also be operationalized for whatever reason with temporal data. 


#### Metadata
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, Numeric, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point Geometry z m

- Name, Text, NA, NA, Name field

- Description, Text, NA, NA, Length 45

- ID, Short, Numeric, NA, NA, Rank ID proximity based

- Type, Text, NA, NA, Three types snack alcohol food

- TypeCode, Short, Numeric, NA, Three codes 1 2 3

- Price Range, Text, NA, NA, Four price ranges: $5-10 $12 $10-15 $15-25

- PriceType, Short, Numeric, NA, Three types 1 2 3

- Veg, Text, NA, NA, Two type codes 00-nonvegetarian 01-vegetarian

```{code}
:tags: ["hide-input"]


import geopandas as gpd
import folium

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)
#border: 2px solid black; background-color: white;


events = "https://github.com/mohamadyassin/myst_airc/releases/download/data/StadiumsandEncounters.geojson"
events = gpd.read_file(events)
if events.crs != "EPSG:3857":
    events = events.to_crs(epsg=3857)

events.explore(m=m,
    style_kwds={"color": "pink", "weight": 3, "fill_opacity": 0.3},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

fnb = "https://github.com/mohamadyassin/myst_airc/releases/download/data/FoodandBeverage.geojson"
fnb = gpd.read_file(fnb)
if fnb.crs != "EPSG:3857":
    fnb = fnb.to_crs(epsg=3857)


fnb.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-utensils" style="color:yellow; background-color:green; border: 3px solid blue;  font-size:12px"></i>')),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

nodes = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Nodes.geojson"
nodes = gpd.read_file(nodes)
if nodes.crs != "EPSG:3857":
    nodes = nodes.to_crs(epsg=3857)

nodes.explore(m=m,
    marker_kwds={
        "radius": 6,           # Adjust the radius as needed
        "fill": True,          # Enable filling
        "fillColor": "yellow", # Set fill color to bright yellow
        "color": "red",      # Set outline color to red
        "weight": 2,          # Adjust outline width as needed
    },
    name="Your Points Layer", # Name for the layer controltiles="Esri.WorldImagery",
    zoom_start=17  
)


m
```

Spatio-Temporal Example

Vehicle Pass Form
Usually at parks or campuses, they have limited access to vehicle parking for supplier and contractors. Each has a mechanism to enforce vehicle traffic. 

I created a smart survey for my geodatabase using Survey123 from Esri. This survey tracks both the agent device location and parking/destination location. Through this app, park agents can issue vehicle passes and capture live information directly in the cloud. 

Then, we can georeference destination locations for real-time visualization. Or we can setup rules to limit issuing vehicle passes for each area based on capacity for example. Notice the unique attributes associated with particular parking locations as you can see in the below video demonstration. 

This survey can be accessed through any smart device using a web browser without the need for registering an account. So, it's very easy to setup and implement. It automatically captures the location of the agent device, date, and time.  It also has domains for error mitigation. 


Link: 
https://arcg.is/1vnq5y1


QR code:


## Code blocks

```{code}
:tags: ["hide-input"]


import leafmap.foliumap as leafmap

m = leafmap.Map(center=[40, -100], zoom=4)
m
```




## Code blocks

```{code}
:linenos:
:emphasize-lines: 3,4

import leafmap.foliumap as leafmap

m = leafmap.Map(center=[40, -100], zoom=4)
m
```

## Conclusion

There are many opportunities to improve open-science communication, to make it more interactive, accessible, more reproducible, and both produce and use structured data throughout the research-writing process. The `mystjs` ecosystem of tools is designed with structured data at its core. We would love if you gave it a try -- learn to get started at <https://myst.tools>.

[2i2c]: https://2i2c.org/
[curvenote]: https://curvenote.com
[docutils]: https://docutils.sourceforge.io/
[executablebooks]: https://executablebooks.org/
[jupyterbook]: https://jupyterbook.org/
[jupyterlab-myst]: https://github.com/jupyter-book/jupyterlab-myst
[sphinx]: https://www.sphinx-doc.org/

```

```
