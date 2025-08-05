---
title: Geospatial Data Science
author: Mohamad Yassin
date: 2022-11-19
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

I selected **SeaWorld San Diego** as my study area and collected **7 feature layers**, integrating them with an imagery basemap using **ArcGIS Pro** and **ArcGIS Online**.

This project covered:

- Spatial and temporal GIS aspects
- Cartography and symbology
- Geodatabase design
- Network data
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

Each feature is treated as a class (Object-Oriented concept) with structured attributes. Metadata is summarized below.

> :::{note}
> Domains and subtypes were applied to restrict data entry errors.
> :::

---

## Walkways Network

- ~2 hours of GPS-based data collection
- Manual redrawing of walkways (~72 lines)
- ~47 turns / 55 junctions
- Accuracy: ~12–17 feet (acceptable for demo)

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


- 11 mapped (likely more in reality)
- Limited attributes for this study

### Metadata

| Field Name | Type   | Description           |
|------------|--------|------------------------|
| ID         | Short  | Proximity rank         |
| Type       | Text   | Default: "bathroom"    |

---

## Rides

- 11 rides mapped
- Attributes include age groups, wait times, and average durations

### Metadata

| Field Name   | Type   | Description                      |
|--------------|--------|-----------------------------------|
| AgeGroup     | Text   | Kids / Adults / Both             |
| AgeGroupType | Short  | Classification index             |
| AverageTime  | Text   | Estimated wait time              |

---

## Food & Beverage (F&B)

- Created for wayfinding and user filtering
- Attributes include type, price, and vegetarian options

### Metadata

| Field Name   | Type   | Description             |
|--------------|--------|--------------------------|
| Type         | Text   | Snack, Alcohol, Food     |
| Price Range  | Text   | $5–10, $10–15, etc.      |
| Veg          | Text   | 00 = Non-Veg, 01 = Veg   |

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

### Additional Case Study

**Spatio-temporal Data Streaming with Affinity Propagation**  
🎥 [Watch video presentation](https://cartogis.org/docs/autocarto/2020/docs/presentations/4c%20Spatio-temporal%20data%20streaming%20with%20affinity%20propagation.mp4)

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
  *(Insert YouTube or video links if available)*

---


---

## Interactive Maps

### 🗺️ Walkways Network

```{code-cell} js
:tags: [map]
:hide-input:
import maplibregl from "maplibre-gl";

const map = new maplibregl.Map({
  container: "walkways-map",
  style: "https://demotiles.maplibre.org/style.json",
  center: [-117.226, 32.764],
  zoom: 17,
});
```

<div id="walkways-map" style="height: 400px;"></div>

---

### 🗺️ Points of Interest (POIs)

```{code-cell} js
:tags: [map]
:hide-input:
const poiMap = new maplibregl.Map({
  container: "poi-map",
  style: "https://demotiles.maplibre.org/style.json",
  center: [-117.226, 32.764],
  zoom: 17,
});
```

<div id="poi-map" style="height: 400px;"></div>

---

### 🗺️ Stadiums and Encounters

```{code-cell} js
:tags: [map]
:hide-input:
const stadiumMap = new maplibregl.Map({
  container: "stadium-map",
  style: "https://demotiles.maplibre.org/style.json",
  center: [-117.226, 32.764],
  zoom: 17,
});
```

<div id="stadium-map" style="height: 400px;"></div>

---



---

## Bathrooms

One of the most frequent questions asked by visitors: "where's the bathroom?" 

I counted 11 bathrooms but I believe that there are more. I bet that you can find a one easily within a short distance when visiting the park.

There aren't a whole lot of attributes for this feature class in my database. But, this depends on the objective of you feature class.

For my case it was just for symbology and wayfinding. But it can also be operationalized with a temporal aspect. For example, we can add date and cleaning attributes to be updated 4 time per day for each location.  We can also plug cleaning supplies information into our attributes.


Metadata 
Field Name, Data Type, Numeric Format, Domain, Description

- OBJECTID, Object ID, NA, NA, Global ID

- Shape, Geometry, NA, NA, Point geometry

- ID, Short, Numeric, NA, NA, Rank ID proximity based

- Type, Text, NA, NA, Default bathroom

---

## Rides

The rides had an interesting design. They were distributed along horizontal line bound SE-NW. 

I counted 11 rides. And I was able to create a template for a static display of ride information and attributes. 

The same information can also be dynamic in a web app. And the attributes can be edited and updated as necessary. 

The attributes were arbitrarily created to help demonstrate how to create a classification system in a web map to aid visitors in making decisions for which rides they'd want to visit first.


Metadata
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

---

## Example of Static Map

Below is an example of a template displaying static maps with attributes info from the dataset. The information and styling in the map template aren't completed. It is sufficient for a demonstration purpose.

---

## Food & Beverage (F&B)

Walking the whole park isn't going to be easy without stopping for food. SeaWorld has a lot of options around nearly every junction. 

The F&B feature class was created for visualizing and wayfinding for this case study. However, I created some basic attributes to help visitors narrow down their F&B choices based on price and vegetarian food options for example. This feature class can also be operationalized for whatever reason with temporal data. 


Metadata
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

---

## Spatio-Temporal Example

### Vehicle Pass Form

Usually at parks or campuses, they have limited access to vehicle parking for supplier and contractors. Each has a mechanism to enforce vehicle traffic. 

I created a smart survey for my geodatabase using Survey123 from Esri. This survey tracks both the agent device location and parking/destination location. Through this app, park agents can issue vehicle passes and capture live information directly in the cloud. 

Then, we can georeference destination locations for real-time visualization. Or we can setup rules to limit issuing vehicle passes for each area based on capacity for example. Notice the unique attributes associated with particular parking locations as you can see in the below video demonstration. 

This survey can be accessed through any smart device using a web browser without the need for registering an account. So, it's very easy to setup and implement. It automatically captures the location of the agent device, date, and time.  It also has domains for error mitigation. 


Link: 
https://arcg.is/1vnq5y1

QR code:

---

## Video demonstration

---

## Additional Spatio-Temporal Case Study Example

Spatio-temporal Data Streaming with Affinity Propagation  
By Nasrin E. Ivaria, Monica Wachowicz, Tamara Agnew, and Patricia A.H. Williams

https://cartogis.org/docs/autocarto/2020/docs/presentations/4c%20Spatio-temporal%20data%20streaming%20with%20affinity%20propagation.mp4

---

## Data Science & Machine Learning

I'm not going to mention the numerous data science and AI applications in geographic sciences or the case studies and applications developed by Esri scientists and researchers over the past few years. You can look at these on your own. 

Spatial Data Science or AI are still new topics. They are great fields to combine because nowadays there are lots of commercial and open source tools that allow data science and AI processes to run from anywhere without the need for a data center or super computers on site.  But please keep in mind that there is a lot of computer science is in these fields. So, you need to be comfortable with coding. 

Now, I'd like to share with you some of the lessons I learned during my personal data science journey over the past few years. But first, I'd like to mention this: if you only worked on a software platform such as Esri's; If you've configured or edited parameters for some of their  advanced analysis methods; if you used their data management tools; then you already know programming. So, you'll be okay if you decided to pick up a programming language such as Python or JavaScript. If you don't know which language to start with then begin with Python. That's what I did. Anyone in data science or AI will tell you this. But, I'd emphasize on this: make sure to learn OOP and social programming. 

I started my journey by learning R from biostatisticians. Which gave me a strong statistical background. Then I learned SQL before moving to Python. I said 'moving' because I haven't used R since then. I became proficient in many data science and AI methods over the past few years. And I was able to pick up many tools necessary to build useful applications. However, when I wanted to make a portfolio of what I learned -I ended up with a bunch of code blocks. 

Which are not good for any practical use on their own. I needed a host for my code and certain ways to organize and serve it. That's why I then found it necessary to learn new concepts in adjacent areas over the years.

JavaScript was gibberish until I learned OOP. Well, it still is. I haven't picked up JS but now I understand how it works from a OOP coding perspective. To be honest, it seems almost inevitable to me that I have to learn JS at some point. Because everytime I learn something new there's  this platform that can serve it right away with JS. I think everything is being stored in the cloud and served over the web nowadays. 

If you're doing school projects or research then a Jupyter notebook will be great for you. However, if you want to build apps and APIs then you'd need OOP. This way you can create classes which will allow you to shape your data and methods as you wish. Just like I did in my feature classes above. You can define attributes and methods for each of your classes. Then you can also code subtypes and create domains and to classify your data and mitigate data entry errors.

You may ask: how do all of the geodatabase stuff above relate to data science or machine learning? Well, you can add attributes such as live video, audio, or  deploy a multitude of sensors to your spatio-temporal datasets. Then you can process and model the data according to your needs. 

All the nonspatial attributes in the data can be processed with data science tools such as Pandas, NumPy, Scikit learn, Keras, PyTorch, etc. Then we can use georeferencing to tie them back to your geometry. Nonetheless, Python also has powerful  GIS libraries such as  GeoPandas  or  PyGIS . You can also find open source platforms.  And if you want to dig deeper go to the  Open Geospatial Consortium (OGS)  for standards and best practices. These are plenty of resources to get you started in geospatial data science.

If you want to implement geospatial data science and machine learning at a school campus; a theme park; or at a similar entity, then you'd have to start from your objective. My suggested approach is first to identify the features or classes that may contain the data necessary for you to achieve your goals. Then build a single geodatabase with a connection to cloud computing and a server that are capable of ingesting and serving your data. Then you can start populating your database by: pulling from existing data sources; digitizing existing manual data collection or legacy systems; or creating new data sources. 

---

## Related Videos

01 / 06

Geospatial Data Scientist -Esri

Geospatial Data Scientist

---

