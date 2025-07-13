---
{"dg-publish":true,"permalink":"/personal/pavlov3-d/pavlov3d-readme-md/","noteIcon":"","created":"2025-07-12T22:23:53.883-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 12\|2025 07-July 12]]


# Pavlov25

Pavlov25 is a modular Python-based data processing and 3D visualization pipeline designed to transform time-series CSV data into structured 3D assets. At its core, it connects:

- **Data Ingestion:** Using Python and Pandas to manage and manipulate CSV time-series data
    
- **3D Export:** Leveraging the FBX SDK to export nested groups and collections representing data curves in 3D space
    
- **Future Goals:** Enabling real-time 3D visualization using WebGL technologies like Three.js or WebGPU
    

## Overview

The primary goal of Pavlov25 is to create a bridge between raw time-series data and immersive 3D visual representations. This enables users to better analyze, explore, and present complex datasets in a spatial context.

The repository contains several packaging and integration directories designed as portals to the central codebase, facilitating different deployment or export workflows.

## Features

- Robust CSV data handling and processing with Pandas
    
- Modular 3D export pipeline to FBX format using the FBX SDK
    
- Support for grouping and nesting data curves for meaningful 3D organization
    
- Designed with extensibility for live 3D visualization on web platforms
    
- Multiple packaging options to integrate with Blender, FastAPI, and other frameworks
    

## Getting Started

### Prerequisites

- Python 3.11+ recommended
    
- FBX SDK installed and configured (link or instructions)
    
- Pandas library for data manipulation
    
- Optional: Blender for testing FBX import/export
    

### Installation

(Describe how to install dependencies or set up environment)

### Usage

(Examples on how to process CSV files and export FBX)

## Project Structure

- `/core/` — Central Python modules managing data processing and FBX export
    
- `/fastAPI/` — Optional FastAPI integration for serving or processing data
    
- `/blender-add-on/` — Blender integration for visualizing or editing exports
    
- `/powershell-scripts/` — Packaging or deployment scripts for Windows environments
    
- `/docker/` — Docker container setups for isolated builds or deployments
    

## Future Work

- Real-time 3D visualization using Three.js or WebGPU
    
- Web-based interactive dashboards for time-series exploration
    
- Additional export formats for other 3D engines
    

