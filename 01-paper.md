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

m = leafmap.Map(center=[32.7658, -117.2264], zoom=17)
m.add_basemap("Esri.WorldImagery")
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


## Map demo

```{code-cell}
:tags: ["hide-input"]
import leafmap.foliumap as leafmap
import geopandas as gpd

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/FoodandBeverage.geojson"
m = leafmap.Map(center=[32.7658, -117.2264], zoom=17)
point_style = {
    "radius": 5,
    "color": "red",
    "fillOpacity": 0.8,
    "fillColor": "blue",
    "weight": 3,
}
hover_style = {"fillColor": "yellow", "fillOpacity": 1.0}
m.add_geojson(
    url, point_style=point_style, hover_style=hover_style, layer_name="FnB"
)

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
