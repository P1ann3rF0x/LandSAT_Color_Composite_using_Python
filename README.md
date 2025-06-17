# LandSAT_Color_Composite_using_Python

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Landsat 8](https://img.shields.io/badge/Landsat%208-USGS-blue.svg)](https://landsat.usgs.gov/landsat-8)

🛰️ Landsat False Color Composite Generation
Dual Approach: Local Processing & Google Earth Engine API

This repository presents two distinct approaches for generating False Color Composite (FCC) images from Landsat satellite data. One method leverages locally stored Landsat imagery, processing it directly using Python libraries, while the other demonstrates the power and efficiency of fetching and processing Landsat data via the Google Earth Engine (GEE) Python API. Both notebooks are designed for educational purposes, illustrating fundamental concepts in remote sensing and geospatial data handling.
✨ Overview

False Color Composites are invaluable in remote sensing for highlighting specific features on the Earth's surface that are not readily visible in true color. By assigning non-visible bands (like Near-Infrared) to visible color channels (like Red), FCCs can effectively differentiate between vegetation health, water bodies, urban areas, and bare soil. This project provides practical examples of how to generate these composites using two common workflows:

    Local Data Processing (LandSAT_local(1).ipynb): Ideal for working with pre-downloaded or smaller datasets, providing granular control over the processing steps.

    Google Earth Engine API (LandSAT_FCC(16_06_25).ipynb): Best suited for large-scale analysis, accessing vast amounts of satellite data on the fly without local storage constraints, and leveraging cloud computing power.
    
🔑 Key Features



 Local Processing Notebook:
     
   📂 Reads individual Landsat bands from local TIFF files.
          
   🔢 Implements custom band normalization for consistent visualization.
  
   🎨 Stacks selected bands (e.g., NIR-Red-Green for vegetation analysis) to create FCCs.
  
   🖼️ Utilizes matplotlib for plotting and visualizing the generated composites.

   

Google Earth Engine (GEE) API Notebook:

   ☁️ Connects to the Google Earth Engine data catalog to access Landsat 8 imagery.

   🌍 Leverages geemap for interactive mapping and visualization within Jupyter notebooks.

   🖥️ Demonstrates cloud-based processing for efficient data retrieval and composite generation.

   🔄 Allows for dynamic selection of imagery based on date and location (implied by GEE capabilities).
   

🛠️ Technical Stack

Software & Libraries

   Python 3.x: The primary programming language.

   Jupyter Notebook: For interactive code execution and documentation.

   LandSAT_local(1).ipynb Dependencies:

        rasterio: For reading and writing raster datasets (e.g., TIFF files).

        geopandas: For working with geospatial vector data (though primarily used for environment setup in the snippet, useful for further analysis).

        numpy: For numerical operations and array manipulation.

        matplotlib: For creating static, interactive, and animated visualizations in Python.

  LandSAT_FCC(16_06_25).ipynb Dependencies:

        earthengine-api: The official Google Earth Engine Python API.

        geemap: A Python package for interactive geospatial analysis with Google Earth Engine, built upon ipyleaflet and folium.

Data Acquisition

   Local Processing: Assumes pre-downloaded Landsat 8 Level 1TP (Tier 1) imagery, typically in .TIF format, with individual bands. The provided notebook expects them in a /content/ directory.

        Example data path structure: /content/LC08_L1TP_144054_20250124_20250130_02_T1_B*.TIF

   Google Earth Engine API: Data is streamed directly from the Landsat collection within the Google Earth Engine data catalog. No local download is required.

🚀 Quick Start

To run these notebooks and generate your own Landsat FCCs, follow these steps:
Prerequisites

   For Local Processing: Ensure you have Python installed and the required libraries (rasterio, geopandas, numpy, matplotlib). You will also need to download Landsat 8 imagery for your area of interest (e.g., from USGS EarthExplorer) and place the band files in a designated local directory.

  For GEE API: You need a Google Earth Engine account. Visit earthengine.google.com to sign up and get access. Ensure you have the earthengine-api and geemap libraries installed in your Python environment.

1. Clone the Repository

git clone https://github.com/your-username/Landsat-FCC-Generation.git
cd Landsat-FCC-Generation

2. Install Dependencies

It's recommended to use a virtual environment.

python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
pip install -r requirements.txt # (You would create this file based on the dependencies listed above)

Alternatively, for the specific libraries used:

# For Local Processing Notebook
pip install rasterio geopandas matplotlib numpy

# For GEE API Notebook
pip install earthengine-api geemap

3. Run Jupyter Notebooks

Launch Jupyter Notebook from the repository root:

jupyter notebook

This will open a browser window with the Jupyter interface.

   For Local Processing: Open LandSAT_local(1).ipynb.

   Place your downloaded Landsat .TIF band files (B2, B3, B4, B5) into the /content/ directory (or update the file paths in the notebook to match your local setup).

   Run all cells sequentially.

   For GEE API: Open LandSAT_FCC(16_06_25).ipynb.

   Ensure your GEE authentication is set up (run ee.Authenticate() and ee.Initialize() if prompted or for first-time use).

   Run all cells sequentially. The interactive map will appear in the notebook output.

📝 Example Code Snippets

Local Processing (LandSAT_local(1).ipynb)
  
    import rasterio
    import numpy as np
    import matplotlib.pyplot as plt
    
    # Load individual bands (adjust paths to your local files)
    GREEN = rasterio.open('/content/LC08_L1TP_144054_20250124_20250130_02_T1_B3.TIF').read(1).astype('float32')
    RED = rasterio.open('/content/LC08_L1TP_144054_20250124_20250130_02_T1_B4.TIF').read(1).astype('float32')
    NIR = rasterio.open('/content/LC08_L1TP_144054_20250124_20250130_02_T1_B5.TIF').read(1).astype('float32')
    
    # Simple normalization function
    def normalize(band):
        return (band - np.min(band)) / (np.max(band) - np.min(band))
    
    # Normalize bands
    red_norm = normalize(RED)
    green_norm = normalize(GREEN)
    nir_norm = normalize(NIR)
    
    # Stack bands as FCC (NIR in Red channel, Red in Green, Green in Blue)
    # This is a common FCC for vegetation analysis (often called "standard false color")
    fcc_image = np.dstack((nir_norm, red_norm, green_norm))
    
    # Plot the FCC
    plt.figure(figsize=(10, 10))
    plt.imshow(fcc_image)
    plt.title('False Color Composite (NIR - Red - Green)')
    plt.axis('off')
    plt.show()

Google Earth Engine API (LandSAT_FCC(16_06_25).ipynb)

    import ee
    import geemap
    
    # Initialize the Earth Engine API (first time only)
    try:
        ee.Initialize()
    except Exception as e:
        ee.Authenticate()
        ee.Initialize()
    
    # Create an interactive map
    Map = geemap.Map(center=[20.2283, 78.9074], zoom=8)
    
    # Define an area of interest (e.g., a simple rectangle)
    # You would define your actual area of interest here or use a geometry drawn on the map
    # For example: aoi = ee.Geometry.Point([78.9074, 20.2283]).buffer(10000)
    
    # Load Landsat 8 image collection, filter by date and cloud cover
    image = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR') \
        .filterDate('2025-01-01', '2025-01-31') \
        .filterBounds(Map.getCenter()) \
        .sort('CLOUD_COVER') \
        .first()
    
    # Define visualization parameters for False Color Composite (NIR, Red, Green)
    # This band combination (B5, B4, B3 for Landsat 8 Surface Reflectance) is common for FCC
    vis_params = {
        'bands': ['B5', 'B4', 'B3'],
        'min': 0,
        'max': 3000,
        'gamma': 1.4
    }
    
    # Add the FCC layer to the map
    if image:
        Map.addLayer(image, vis_params, 'Landsat 8 FCC')
        Map.centerObject(image, 9) # Center map on the image
    else:
        print("No image found for the specified criteria.")
    
    # Display the map
    Map

🎯 Applications

This work can be applied to various remote sensing and GIS tasks, including:

   Vegetation Health Monitoring: FCCs are excellent for assessing plant vigor and detecting changes over time.

   Land Use/Land Cover Mapping: Differentiating between various land cover types like forests, agricultural areas, urban sprawl, and water bodies.

   Environmental Monitoring: Tracking deforestation, urbanization, and disaster impacts.

   Agricultural Analysis: Monitoring crop growth stages, identifying stress, and yield prediction.

   Water Resource Management: Mapping water body extents and quality.

⚠️ Limitations and Considerations

Local Processing:

   Requires manual download and local storage of large satellite image files.

   Computationally intensive for very large areas or time series analysis, dependent on local hardware.

   Requires careful path management for individual band files.

Google Earth Engine API:

   Requires an internet connection and a GEE account.

   Steeper learning curve for users unfamiliar with JavaScript/Python and GEE's API concepts.

   GEE's data access and processing capabilities are subject to its platform policies and rate limits.

   General: Image quality (cloud cover, atmospheric conditions) can significantly affect the clarity and interpretability of FCCs.

🔮 Future Enhancements

   Interactive Band Selection: Implement widgets in the local processing notebook to allow users to dynamically select bands for FCC generation.

   Time Series Analysis: Extend both notebooks to allow for the generation of FCCs over a time series to observe changes.

   Cloud Masking: Incorporate more robust cloud masking techniques for cleaner imagery in both approaches.

   User Interface: Develop a simple web application using libraries like Streamlit or Flask to provide a more user-friendly interface for FCC generation without requiring direct code interaction.

   Comparison Tool: Add functionality to visually compare FCCs generated by both local and GEE methods.

📚 References

   Landsat Program: https://landsat.gsfc.nasa.gov/

   Google Earth Engine: https://earthengine.google.com/

   geemap Documentation: https://geemap.org/

   rasterio Documentation: https://rasterio.readthedocs.io/en/latest/

🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to fork the repository and submit pull requests.
📧 Contact

For questions or suggestions, please contact:

    Author: Udhay Kiraan K H

    eMail: udhay_k@ce.iitr.ac.in

    Institution: Indian Institute of Technology, Roorkee

Note: The conceptual code snippets provided are illustrative. Please refer to the respective Jupyter Notebook files (LandSAT_local(1).ipynb and LandSAT_FCC(16_06_25).ipynb) for the full and runnable code.
