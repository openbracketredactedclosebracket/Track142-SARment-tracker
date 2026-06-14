# SARment-tracker
Research project on processing time-series SAR imagery to track limestone mining for cement production.
Based on methodology by Zefa Yang et. al. (2021), DOI: https://doi.org/10.1109/TGRS.2021.3055854 
                        "Prediction of Mining-Induced Kinematic 3-D Displacements From InSAR Using a Weibull Model and a Kalman Filter"\
\
**Data Citation**
Buzzanga, Brett, et al. "Toward sustained monitoring of subsidence at the coast using InSAR and GPS: An application in Hampton Roads, Virginia." Geophysical Research Letters 47.18 (2020): e2020GL090013 \
(https://www.earthdata.nasa.gov/data/projects/aria) \
\
**Programs Used:** \
ARIA-Tools (https://github.com/aria-tools/ARIA-tools) - Creating interferogram stack in preparation for time-series analysis \
Mintpy (https://github.com/insarlab/mintpy) - Processing stack to produce time-series velocity and other data attributes of the stack \
ArcGIS Pro (https://www.esri.com/en-us/arcgis/products/arcgis-pro/overview) - Manipulation and analysis of output files in the AOI \
\
**General Workflow:**
1) Download ARIA-GUNW Sentinel-1 Data from ASF Vertex Data Search (or with aria-tools package). In this study, Track 142, Frame 22013 was used. Each downloaded image is an interferogram pair with time interval 7-21 days between the two parts of the pair. ARIA data may also be processed and downloaded in the terminal using aria-tools. (https://hyp3-docs.asf.alaska.edu/guides/gunw_product_guide/). 
See [ARIA File Names](ARIA_filenames.txt) for full list of files used. 

<img width="1747" height="968" alt="image" src="https://github.com/user-attachments/assets/b50ca696-d276-420d-afa2-6c2f9c68c15f" /> <img width="840" height="357" alt="image" src="https://github.com/user-attachments/assets/5eea652a-4fb2-4b30-a7a0-916ca6149b88" /> 
2) Follow the instructions here: https://github.com/aria-tools/ARIA-tools#installation to download conda and mamba and create an environment in the terminal for using aria-tools package.  

3) In the directory containing the downloaded ARIA-GUNW *.nc files (files should look like: S1-GUNW-A-R-142-tops-20150811_20150730-101055-00116E_00030N-PP-7dbd-v3_0_1.nc), run the aria-tools program ariaTSsetup.py:
   ```
   ariaTSsetup.py -f "./*.nc" -w ./STACK_DIR_NAME --bbox "BOUNDING_BOX_COORDS"
   ```
   Perform in the directory containing all the downloaded .nc files. Bounding box (--bbox) optional. The output directory should have multiple subdirectories, such as azimuthAngle, bPerpendicular, coherence, connectedComponents, DEM, stack, etc.
4) Download Mintpy in the same environment as aria-tools by following the instructions here:
5) In the newly created STACK_DIR_NAME stack directory, run ```smallbaselineApp.py -g``` to generate a time-series analysis template (smallbaselineApp.cfg should appear in your STACK_DIR_NAME). Before running ```smallbaselineApp.py```, you may need to change some defaults in the smallbaselineApp.cfg file to ensure the correct files in the STACK_DIR_NAME are being referenced by the program. See [Mintpy configuration](smallbaselineApp.cfg) for the configuration used. Once the default configuration is modified, you can run smallbaselineApp.cfg. You may also download the configuration as a .txt file and run ```smallbaselineApp.py CONFIG_NAME.txt``` to run with the modified configurations. Skip the tropospheric step when running.
6) Check the newly created `pic` folder to see output graphs. You may also use graph.py to view and save outputs, such as [velocity.tif](Output_Data/velocity.tif).
7) In ArcGIS, an Area of Interest can be specified to view coherence and velocity in a specific mining area.
