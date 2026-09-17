---
layout: page
title: Zurich Mobility Accessibility App
description: A web application tracking real-time station accessibility and accessible parking spaces across Zurich.
img: assets/img/projects/accessibility-preview.png
importance: 2
category: coursework
github: https://github.com/igor-martinelli/ETH-Fundamentals-Web-Engineering/tree/main/Final%20Project
---

### Overview

Navigating urban public transport presents unique challenges for individuals with disabilities or reduced mobility. Developed as part of the **Fundamentals of Web Engineering** course at **ETH Zurich**, this centralized web application provides real-time, accessible transport information for the city of Zürich.

The platform aggregates open city data to deliver a streamlined interface that allows users to assess station accessibility (BEHIG compliance) and locate designated disabled parking spots near transit hubs.

---

### Dashboard & Core Features

{% include figure.liquid 
  loading="eager" 
  path="assets/img/projects/accessibility-dashboard.png" 
  title="Complete Application Dashboard" 
  class="img-fluid rounded z-depth-1" 
  zoomable=true 
%}

The main dashboard integrates an interactive map with search filters and dynamic status panels. Users can inspect public transit hubs across Zurich to evaluate accessibility metrics at a glance before planning their journeys.

---

### Station Accessibility Details

{% include figure.liquid 
  loading="eager" 
  path="assets/img/projects/accessibility-train.png" 
  title="Station Accessibility Markers" 
  class="img-fluid rounded z-depth-1" 
  zoomable=true 
%}

Zooming into specific train and tram stations reveals detailed accessibility indicators. Custom map markers display clear icon labels showing whether a station features wheelchair-accessible ramps, step-free platform access, and accessible restroom facilities.

---

### Accessible Parking Proximity

{% include figure.liquid 
  loading="eager" 
  path="assets/img/projects/accessibility-parking.png" 
  title="Nearby Parking Highlights" 
  class="img-fluid rounded z-depth-1" 
  zoomable=true 
%}

Selecting a station triggers a spatial search that highlights surrounding handicap-accessible parking spaces. The system automatically identifies and highlights the closest parking spots in yellow, helping drivers with reduced mobility quickly locate optimal parking options near transit links.

---

### Tech Stack & Datasets

* **Open Data Sources**: Integrated official city feeds, including *Behindertenparkplätze* (Open Data Swiss), *Bestandsaufnahme BEHIG* (station accessibility audits), and *VBZ GTFS* transit schedules.
* **Architecture**: Responsive web frontend hosted on Vercel for fast, accessible performance across mobile and desktop devices.

---

### Repository

* **GitHub Repository**: [ETH-Fundamentals-Web-Engineering Source Code](https://github.com/igor-martinelli/ETH-Fundamentals-Web-Engineering/tree/main/Final%20Project)