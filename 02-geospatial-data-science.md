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

# Geospatial Data Science  
_A case study in modern geodatabase_

> **Author:** Mohamad Yassin  
> **Class:** GIS III – Geodatabase Design  
> **Date:** November 19, 2022

---

## Introduction

### How did we get here?

Geospatial isn’t only about geographic sciences, but also about data and information technologies — especially since the introduction of GNSS (global navigation satellite systems). However, spatial and informational technologies were often siloed.

The history of Earth measurement goes back to a famous disagreement: Cassini (France) argued Earth was elongated, while Newton believed it was flattened. Expeditions from 1735 to 1736 eventually confirmed Newton’s theory, without him ever leaving his desk.

From the early 1800s with the **Survey of the Coast** under Jefferson, to the 1960s when satellite-based surveys began, geodesy evolved into today's **National Geodetic Survey (NGS)** under NOAA.

With today’s access to powerful tools, anyone can conduct spatial analysis from their desk — just like I did for this case study.

> _Much of this historical background is drawn from lectures by Dave Doyle (NGS, Retired). See videos in the “Resources” section below._

---

## Geodatabase Project

### Objective

Build a geodatabase and demonstrate a useful application or service.

I went on a trip to **SeaWorld San Diego** as my study area and collected **7 feature layers**, integrating them with an imagery basemap using **ArcGIS Pro** and **ArcGIS Online**.

This project covered:

- Spatial and temporal GIS aspects
- Cartography and symbology
- Geodatabase design
- Network dataset
- Data collection
- Data science & AI use cases

> _All data used are fictional or publicly collected for educational demonstration._

---

## SeaWorld San Diego Geodatabase

### Basemap

- Source: Esri imagery
- CRS: WGS84 Web Mercator
- Resolution: 0.3 meters

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

### Feature Layers Overview

Each feature is considered as a class (think OOP) with structured attributes. Metadata is summarized below.

> :::{note}
> Domains and subtypes were applied to restrict data entry errors.
> :::

---

## Walkways Network

- ~2 hours of GPS-based data collection
- Manual redrawing of walkways (~72 lines)
- ~47 turns / 55 junctions
- Accuracy: ~12–17 feet (acceptable for demo)

It took me about 2 hours to walk the whole park and collect longitude and latitude coordinates for my feature classes. I also used Field Maps from Esri briefly to help draw features in the field. 

Furthermore, I spent a few hours on my base map to manually re-draw a walkway network and nodes. Neither horizontal nor vertical accuracy were important for my case study. I probably was able to attain 12-17 feet in general. 

For my walkways network, I ended up with 72 individual walkways -which were designed to look like the maze of an amusement park. 

In ArcGIS you can configure your network dataset properties for cost functions based on length, time or other attributes. You can also add rules, constraints, and directions, amongst other functions for network traffic  control. 

Network datasets usually have many attributes to define travel rules. My walkways feature class only included the geometry  with no further attributes for this case study. Further attribute definitions and properties configurations can be performed at a later time if we desire to create applications. 

I completed a  Network Analyst tutorial in ArcGIS Pro  before creating my own network dataset. In the tutorial you can look at the data attributes which can help decide which ones to choose when creating your own road or walkway network based on a predefined objective. Or you can look at smart city networks for a perspective about network datasets and systems in general.  


> _No vertical accuracy required. Just topological for demo._

### Metadata (Sample)

| Field Name | Type      | Description           |
|------------|-----------|------------------------|
| OBJECTID   | Object ID | Unique Global ID       |
| Shape      | Geometry  | Polyline geometry (ZM) |

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
---

## Nodes

Created manually for visualizing junctions.

For my walkway network to perform properly if I wanted to create a direction app, I'd need to setup my roads based on the number of junctions in the network. 

There's a tool in ArcGIS Pro that will create the network dataset for you.  It includes a separate junction dataset by default. The junctions help with analyzing directions or finding best routes in a network. I created this nodes feature class manually just for visual purposes for now. 

I ended up with 55 junctions and dead ends. So, in short: my network has 72 walkways and 47 turns. Inside the area of approximately a square quarter mile. 

### Metadata

| Field Name | Type      | Description        |
|------------|-----------|---------------------|
| OBJECTID   | Object ID | Global ID           |
| Shape      | Geometry  | Point geometry      |

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
---

## Points of Interest (POIs)

Classified into 4 subtypes:
1. Vehicle traffic
2. Customer service
3. Landmarks
4. Smoking areas

One of the first things visitors need to know in order to orient themselves are POI. 

The POI is a dataset that includes four feature subtypes. 

Vehicle traffic locations. Which include driving through the tolls into the large parking lots and the disabled and VIP parking areas symbolized near the park entrance. 
Points that indicate customer service interactions: Reservations, Sea Pixels, and Guest Services. 
Landmark points: park entrance and the Skyride Tower.
The two smoking areas. 
Subtype classification was used for symbology purposes in my case study. But this can also be used for operationalizing your data. 

Each of the four different subtypes  I defined can include a unique temporal dataset associated with it. For example, the vehicle traffic feature can include daily vehicle log information attached to them (see my survey example in the next section). 

> Subtypes support temporal data logging.

### Metadata

| Field Name | Type   | Description                            |
|------------|--------|----------------------------------------|
| Name       | Text   | POI name                               |
| ID         | Short  | Rank/index                             |
| *Type*     | Short  | Subtype: 0=Landmark, 1=Vehicle, etc.   |

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
---

## Stadiums and Encounters

- Represented as polygons
- Ideal for spatio-temporal tracking (e.g., animal monitoring, temperature, safety)

  Stadiums and encounters are at the core of SeaWorld San Diego. They include entertainment and education experiences for all ages. You can see on the map how they are strategically distributed across the entire park. 

Experiences polygons were created for visual aid. But we can also look at them from a spatio-temporal perspective. This is one of the smartest and most widely used applications for GIS.  

The data can take a temporal aspect to measure the change. Instead of a passive point or area in space. We can measure the change in the non-spatial attributes. We can embed sensors, cameras, or any other method to collect data which also has spatial coordinates.

For instance, the zoological department can install sensors to or manually enter data into the geodatabase to monitor animals, and to measure temperatures, or health and safety indicators for the welfare of their animals. 

These attributes can be updated as frequently as desired. This is similar to what any web app database con do. But the spatial (geometry) attributes can help us further analyze or relate spatial phenomenon or analysis to our data. 


### Metadata

| Field Name | Type   | Description           |
|------------|--------|------------------------|
| Name       | Text   | Encounter/stadium name |
| Type       | Text   | Event/Presentation     |
| Duration   | Text   | Show length            |
| VIP        | Text   | Domain-based           |

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
---


## Bathrooms

One of the most frequent questions asked by visitors: "where's the bathroom?" 

I counted 11 bathrooms but I believe that there are more. I bet that you can find a one easily within a short distance when visiting the park.

There aren't a whole lot of attributes for this feature class in my database. But, this depends on the objective of you feature class.

For my case it was just for symbology and wayfinding. But it can also be operationalized with a temporal aspect. For example, we can add date and cleaning attributes to be updated 4 time per day for each location.  We can also plug cleaning supplies information into our attributes.

### Metadata 

Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID  
- Shape, Geometry, NA, NA, Point geometry  
- ID, Short, Numeric, NA, NA, Rank ID proximity based  
- Type, Text, NA, NA, Default bathroom  

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
        "radius": 6,
        "fill": True,
        "fillColor": "yellow",
        "color": "red",
        "weight": 2,
    },
    name="Your Points Layer",
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

m
```


### Metadata

| Field Name | Type   | Description           |
|------------|--------|------------------------|
| ID         | Short  | Proximity rank         |
| Type       | Text   | Default: "bathroom"    |

---

## Rides

- 11 rides mapped
- Attributes include age groups, wait times, and average durations

The rides had an interesting design. They were distributed along horizontal line bound SE-NW. 

I counted 11 rides. And I was able to create a template for a static display of ride information and attributes. 

The same information can also be dynamic in a web app. And the attributes can be edited and updated as necessary. 

The attributes were arbitrarily created to help demonstrate how to create a classification system in a web map to aid visitors in making decisions for which rides they'd want to visit first.


### Metadata

| Field Name   | Type   | Description                      |
|--------------|--------|-----------------------------------|
| AgeGroup     | Text   | Kids / Adults / Both             |
| AgeGroupType | Short  | Classification index             |
| AverageTime  | Text   | Estimated wait time              |

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

# Start from Walkways base

url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

# Load rides layer
rides = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Rides_JSON.geojson"
rides = gpd.read_file(rides).to_crs(epsg=3857)

rides.explore(m=m,
    marker_type="marker",
    marker_kwds=dict(icon=folium.DivIcon(html='<i class="fa-solid fa-rocket" style="color:hotpink; background-color:white; border: 2px solid black; font-size:20px"></i>'))
)

m
```
---

## Food & Beverage (F&B)

- Created for wayfinding and user filtering
- Attributes include type, price, and vegetarian options

Walking the whole park isn't going to be easy without stopping for food. SeaWorld has a lot of options around nearly every junction. 

The F&B feature class was created for visualizing and wayfinding for this case study. However, I created some basic attributes to help visitors narrow down their F&B choices based on price and vegetarian food options for example. This feature class can also be operationalized for whatever reason with temporal data. 

### Metadata

| Field Name   | Type   | Description             |
|--------------|--------|--------------------------|
| Type         | Text   | Snack, Alcohol, Food     |
| Price Range  | Text   | $5–10, $10–15, etc.      |
| Veg          | Text   | 00 = Non-Veg, 01 = Veg   |

```{code-cell}
:tags: ["hide-input"]
import geopandas as gpd
import folium

# Load Walkways base layer
url = "https://github.com/mohamadyassin/myst_airc/releases/download/data/Walkways.geojson"
gdf = gpd.read_file(url)
if gdf.crs != "EPSG:3857":
    gdf = gdf.to_crs(epsg=3857)

m = gdf.explore(
    style_kwds={"color": "lime", "weight": 6},
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

# Load Food & Beverage layer
fnb = "https://github.com/mohamadyassin/myst_airc/releases/download/data/FoodandBeverage.geojson"
fnb = gpd.read_file(fnb)
if fnb.crs != "EPSG:3857":
    fnb = fnb.to_crs(epsg=3857)

# Add F&B points with icon styling
fnb.explore(
    m=m,
    marker_type="marker",
    marker_kwds=dict(
        icon=folium.DivIcon(
            html='<i class="fa-solid fa-utensils" '
                 'style="color:darkred; background-color:white; border: 2px solid black; '
                 'font-size:20px; padding:2px"></i>'
        )
    ),
    tiles="Esri.WorldImagery",
    zoom_start=17  
)

m
```

---

## Spatio-Temporal Example

### Vehicle Pass Form with Survey123

> Uses GPS location + form metadata to issue temporary parking permits in real-time.

- Device captures geolocation, date, and time
- Easy access via QR or link
- No login required

📎 [Survey Link](https://arcg.is/1vnq5y1)

```{figure} ./images/survey-qr.png
---
alt: QR code for survey link
width: 200px
---
Scan to open the live survey
```

---

### Spatio-Temporal Case Study (Sensors)

**Spatio-temporal Data Streaming with Affinity Propagation**  
🎥 [Watch video presentation](https://cartogis.org/docs/autocarto/2020/docs/presentations/4c%20Spatio-temporal%20data%20streaming%20with%20affinity%20propagation.mp4)

Usually at parks or campuses, they have limited access to vehicle parking for supplier and contractors. Each has a mechanism to enforce vehicle traffic. 

I created a smart survey for my geodatabase using Survey123 from Esri. This survey tracks both the agent device location and parking/destination location. Through this app, park agents can issue vehicle passes and capture live information directly in the cloud. 

Then, we can georeference destination locations for real-time visualization. Or we can setup rules to limit issuing vehicle passes for each area based on capacity for example. Notice the unique attributes associated with particular parking locations as you can see in the below video demonstration. 

This survey can be accessed through any smart device using a web browser without the need for registering an account. So, it's very easy to setup and implement. It automatically captures the location of the agent device, date, and time.  It also has domains for error mitigation.

---

## Data Science & Machine Learning

Geospatial Data Science is an emerging field powered by:

- Python (Pandas, NumPy, Scikit-learn, PyTorch, GeoPandas)
- Cloud-based environments
- API-driven apps and dashboards

---

### My Learning Journey

- Started with **R** (biostatistics)
- Learned **SQL**
- Now proficient in **Python**  
- Understand **OOP** and app design  
- Planning to learn **JavaScript** for web services

Spatial Data Science or AI are still new topics. They are great fields to combine because nowadays there are lots of commercial and open source tools that allow data science and AI processes to run from anywhere without the need for a data center or super computers on site.  But please keep in mind that there is a lot of computer science is in these fields. So, you need to be comfortable with coding. 

Now, I'd like to share with you some of the lessons I learned during my personal data science journey over the past few years. But first, I'd like to mention this: if you only worked on a software platform such as Esri's; If you've configured or edited parameters for some of their  advanced analysis methods; if you used their data management tools; then you already know programming. So, you'll be okay if you decided to pick up a programming language such as Python or JavaScript. If you don't know which language to start with then begin with Python. That's what I did. Anyone in data science or AI will tell you this. But, I'd emphasize on this: make sure to learn OOP and social programming. 

I started my journey by learning R from biostatisticians. Which gave me a strong statistical background. Then I learned SQL before moving to Python. I said 'moving' because I haven't used R since then. I became proficient in many data science and AI methods over the past few years. And I was able to pick up many tools necessary to build useful applications. However, when I wanted to make a portfolio of what I learned -I ended up with a bunch of code blocks. 

Which are not good for any practical use on their own. I needed a host for my code and certain ways to organize and serve it. That's why I then found it necessary to learn new concepts in adjacent areas over the years.

JavaScript was gibberish until I learned OOP. Well, it still is. I haven't picked up JS but now I understand how it works from a OOP coding perspective. To be honest, it seems almost inevitable to me that I have to learn JS at some point. Because everytime I learn something new there's  this platform that can serve it right away with JS. I think everything is being stored in the cloud and served over the web nowadays. 

If you're doing school projects or research then a Jupyter notebook will be great for you. However, if you want to build apps and APIs then you'd need OOP. This way you can create classes which will allow you to shape your data and methods as you wish. Just like I did in my feature classes above. You can define attributes and methods for each of your classes. Then you can also code subtypes and create domains and to classify your data and mitigate data entry errors.

You may ask: how do all of the geodatabase stuff above relate to data science or machine learning? Well, you can add attributes such as live video, audio, or  deploy a multitude of sensors to your spatio-temporal datasets. Then you can process and model the data according to your needs. 

All the nonspatial attributes in the data can be processed with data science tools such as Pandas, NumPy, Scikit learn, Keras, PyTorch, etc. Then we can use georeferencing to tie them back to your geometry. Nonetheless, Python also has powerful  GIS libraries such as  GeoPandas  or  PyGIS . You can also find open source platforms.  And if you want to dig deeper go to the  Open Geospatial Consortium (OGS)  for standards and best practices. These are plenty of resources to get you started in geospatial data science.

If you want to implement geospatial data science and machine learning at a school campus; a theme park; or at a similar entity, then you'd have to start from your objective. My suggested approach is first to identify the features or classes that may contain the data necessary for you to achieve your goals. Then build a single geodatabase with a connection to cloud computing and a server that are capable of ingesting and serving your data. Then you can start populating your database by: pulling from existing data sources; digitizing existing manual data collection or legacy systems; or creating new data sources. 

> :::{admonition} Pro Tip
> If you've used ArcGIS tools, you're already scripting. Learning Python or JS will be a smooth transition.
> :::

---

## Recommendations

To implement geospatial data science in practice:

1. Define your **objective**
2. Identify necessary **feature classes**
3. Build a **centralized geodatabase**
4. Connect to **cloud + compute resources**
5. Populate it via:
   - Public or legacy datasets
   - Manual digitization
   - New sensor networks

---

## Related Videos

- 🎥 _Geospatial Data Scientist - Esri_  
  *(https://youtu.be/tRpkQa0rXo4?si=-cC5E1WGy8mZw6Ts)*

- 🎥 _What is Spatial Data Science? - Carto_  
  *(https://youtu.be/osAbJeTho5w?si=U99_HHQAH9hUJfZV)*

- 🎥 _Geodetic Surfaces and Datums by Dave Doyle_  
  *(https://youtu.be/VeBRfIu5jZ8?si=rGh1Ot34wRt2L9ya)*

- 🎥 _Horizontal/Geometric Datums by Dave Doyle_  
  *(https://youtu.be/Aubb9RVxqXA?si=1CPBds-eEcXA6ZlF)*

- 🎥 _Vertical Datums by Dave Doyle_  
  *(https://youtu.be/Pym5dLiDB00?si=n9ePlXeq4_iBmRv_)*

- 🎥 _2022 Datum Change by Dave Doyle_  
  *(https://youtu.be/hAG1ahdJejg?si=3AkYSJsl9CgaLikB)*

---


