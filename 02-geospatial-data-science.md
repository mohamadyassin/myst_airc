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

---

## Nodes

Created manually for visualizing junctions.

### Metadata

| Field Name | Type      | Description        |
|------------|-----------|---------------------|
| OBJECTID   | Object ID | Global ID           |
| Shape      | Geometry  | Point geometry      |

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

---

## Bathrooms

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

