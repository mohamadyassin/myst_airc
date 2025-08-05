---
title: Tsunami Awareness
author: Mohamad Yassin
date: 2025-08-04
---

# Tsunami Awareness GIS Project

A couple of months ago, I was facing the decision of choosing a topic for my GIS capstone project — the most nerve-wracking part of the entire GIS certificate program. This final course was meant to showcase all of the skills learned over five quarters (15 months). I had the liberty to choose my topic, which was a challenge on its own.

## Project Requirements

The minimum requirements for this GIS capstone project included:

1. Digitize or edit a study area using the Editor tool  
2. Perform at least 4 geoprocessing tasks (e.g., union, intersect, clip) or raster algebra calculations  
3. Geocode an attribute database or create points from XY coordinates  
4. Run a minimum of 3 attribute queries, including a relational join  
5. Update or calculate values in an attribute field  
6. Create thematic map(s) with standard cartographic elements  
7. Use ArcToolbox and ModelBuilder to automate your geoprocessing model  

---

## Why Tsunami?

As an open-water swimmer for over a year, I’ve come to respect the energy in ocean waves. One way to avoid a crashing wave is by duck diving under it. However, waves on land — like tsunamis — are a different story.

On September 30, 2022, NBC 6 (Miami) aired shocking footage of a rogue wave sweeping pedestrians into the ocean. Watching that moment made me realize how little I knew about tsunami safety, even though I had lived in coastal communities for years.

> [Giant Wave Sweeps People Off Sidewalk](https://www.nbcmiami.com/news/local/giant-wave-sweeps-people-off-sidewalk-near-south-beach-pier-injuring-6/)

This became the inspiration for my GIS capstone — studying tsunami risk in **San Diego County**, and how we can improve public awareness.

---

## San Diego County Tsunami Risk

The **2023 Multi-Jurisdictional Hazard Mitigation Plan** ranked tsunami risk across four categories:

- **Geographic Area Affected**: Negligible (<10% of county)  
- **Maximum Probable Extent**: Weak  
- **Probability of Future Events**: Unlikely (<1% annually)  
- **Overall Significance**: Medium

While the affected area is limited, **coastal zones are densely populated with critical infrastructure**.

> :::{note}
> The evacuation areas in the 2022 *Emergency Operations Plan* used older (2nd generation) CGS inundation maps. The 3rd generation maps released in **November 2022** may require updates to those plans.
> :::

Local offshore faults — **Coronado Bank, San Diego Trough, San Clemente–San Isidro** — present unique risks due to their proximity. Response time from local events could be **under 5 minutes**.

```{figure} ./images/fault-lines-map.png
---
name: fault-lines
width: 100%
alt: Near-shore faults in San Diego County
---
Near-shore fault lines capable of generating tsunamis
```

---

## Public Awareness Survey

Between **May 7–18, 2023**, I conducted a survey to assess public awareness:

- 38% believed a tsunami is "somewhat likely"
- 50% understood the difference between near-shore and far-field sources
- 46% were unaware that **evacuation time from near-shore events could be under 5 minutes**

> :::{admonition} Implication
> Without public education, even the best evacuation plans may fail.
> :::

---

## Conceptual Model: Shelter Suitability

To support awareness and planning, I created a **suitability model** to identify evacuation shelters.

Inputs included:

- Inundation area
- Elevation (DEM)
- Hospitals, schools, fire stations
- Roads

```{figure} ./images/suitability-model.png
---
width: 90%
alt: Raster output for tsunami shelter suitability
---
Suitability raster map output from tsunami model
```

Model outputs were classified into **three classes** and rasterized at ~203m resolution.

---

## Inundation Analysis (2022 CGS Map)

> :::{tip}
> Military bases, shallow water, and the City of Encinitas were excluded from the analysis.
> :::

**Threatened Areas:**

- **San Diego**: Harbor Island, Shelter Island, Ocean Beach, Mission Beach  
- **Coronado**: Coronado City, Silver Strand  
- **Imperial Beach**: Downtown IB  
- **Oceanside**: Coastal properties

---

## Threatened Structures

To identify structures at risk:

- Used **Assessor parcel data**
- Clipped with CGS inundation zones
- Joined attributes to assess land use, value, construction quality, living area, etc.

| City           | Parcels | Occupancy | Acreage | Parcel:Acre |
|----------------|---------|-----------|---------|--------------|
| San Diego      | 7,402   | 648,504   | 7,444   | 0.99         |
| Coronado       | 5,152   | 345,764   | 3,969   | 1.23         |
| Imperial Beach | 1,470   | 131,155   | 1,506   | 0.98         |
| Oceanside      | 4,810   | 59,338    | 681     | 7.06         |

---

## DEM + Parcel Heights

Used 2014 **USGS QL2 LiDAR** clipped to tsunami zones:

- Point spacing: 4
- Spatial resolution: 0.7 meters
- Elevation range: **–10m to 507m**

Joined to parcel polygons to assess:

- Maximum elevation
- Potential horizontal evacuation (30m+ height)
- Building resilience in densely populated, road-limited coastal areas

---

## Schools & Roads

To support evacuation planning:

- Clipped schools with a **1-mile buffer**
- Included **highways, major roads, freeways**

```{figure} ./images/school-evacuation-sites.png
---
width: 100%
alt: Map showing schools and evacuation roads
---
Evacuation map with schools and road networks
```

Clicking on a school or parcel reveals its attributes.

---

## Shelter Selection

Final shelter sites were selected **manually** based on:

- Elevation
- Distance from inundation zones
- Access via major roads

---

## Resources

- 📄 [Full Report (Google Drive)](https://drive.google.com/file/d/1tBmkK8lmoSG9zXFQ261PcQUr3cMKQr-B/view?usp=sharing)
- 📊 [Survey & Dataset (Google Sheets)](https://docs.google.com/spreadsheets/d/13HmziwxzwDMmdBhxaNaADhe1-fWVE7qr)

---

## Next Steps

You can improve the shelter suitability model by adjusting weights for:

- Proximity to critical services
- Traffic capacity
- Land ownership

You can also build this as an **interactive map** using Leaflet or ipyleaflet inside Jupyter Book.

---

