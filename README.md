# Repository: texture_loss_estimation  

This framework and its instructions (all below) make use of QGIS and MATLAB.  



## Instructions: texture_model  

To characterize local texture, we need to:  

- Extract building information  
- Compute building-specific drag coefficients  

### Extracting building information  

We need to:  

- Extract latitude, longitude, footprint area  
- Assign census tract (by spatial join)  
- Export attribute table as .csv  
- Convert .csv to .mat  

#### To extract latitude, longitude, footprint areas:  

We'll be using:  

- Building footprints from Microsoft: [here](https://github.com/microsoft/USBuildingFootprints)  

**Step 1:** Starting QGIS  

- Start QGIS  

**Step 2:** Creating .geojson layer  

- "Layer" > "Add layer" > "Add vector layer"  
- Fill in "Source type": "HTTP(S), cloud, etc."  
- Fill in "Protocol": "GeoJSON"  
- Fill in "URI": Copy-paste path from File Explorer (e.g., C:&#92;...&#92;Building_footprints&#92;Florida.geojson)  
- "Add" and "close" (might take a moment)  

**Step 3:** Extracting latitude  

- "Open field calculator" (abacus icon)  
- "Create new field"  
- Fill in "Output field name": lat  
- Fill in "Output field type": "Decimal number (real)"  
- Fill in "Output field length": 10 (default)  
- Fill in "Precision": 8  
- Fill in "Expression": y($geometry)  
- "Okay" (might take a moment)  

**Step 4:** Extracting longitude  

- "Open field calculator"  
- "Create new field"  
- Fill in "Output field name": lon  
- Fill in "Output field type": "Decimal number (real)"  
- Fill in "Output field length": 10 (default)  
- Fill in "Precision": 8  
- Fill in "Expression": x($geometry)  
- "Okay" (might take a moment)  

**Step 5:** Extracting footprint area  

- "Open field calculator"  
- "Create new field"  
- Fill in "Output field name": area  
- Fill in "Output field type": "Decimal number (real)"  
- Fill in "Output field length": 10 (default)  
- Fill in "Precision": 8  
- Fill in "Expression": $area   
- "Okay" (might take a moment)  

#### To assign census tract (by spatial join):  

We'll be using:  

- Cartographic boundary files from US Census Bureau: [here](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)  

**Step 6:** Converting .geojson to .csv  

- "Layers" (lower LHS) > Right-click on .geojson layer  
- "Export" > "Export layer as"  
- Fill in "Format": "Comma separated value (CSV)"  
- Fill in "File name": Select path from File Explorer (e.g., C:&#92;...&#92;Florida.csv)  
- "Okay" (might take a moment)  
- Remove .geojson layer (it's easier to work with delimited layers)  
- "Layer" > "Add layer" > "Add delimited text layer" &ast;  
- Fill in "File name": Select path from File Explorer &ast;  
- Fill in "x field": lon &ast;  
- Fill in "y field": lat &ast;  
- "Add" and "close" &ast;  
- &ast;These steps might be unnecessary.  

**Step 7:** Creating .shp layer  

- Look up FIPS code (e.g., for Florida it's 12)  
- From File Explorer, drag and drop census tract .shp file  

**Step 8:** Assigning census tract (by spatial join)  

- "Vector" > "Data management tools" > "Join attributes by location"  
- Fill in "Input layer": Select .csv layer  
- Fill in "Join layer": Select .shp layer  
- Fill in "Geometric predicate": "Intersects" (default)  
- Fill in "Join type": "One-to-one"  
- "Run" and "Close" (might take a moment)  

#### To export attribute table as .csv:  

**Step 9:** Exporting .csv layer  

- "Layers" > Right-click on .csv layer  
- "Export" > "Export layer as"  
- Fill in "Format": "Comma separated value (CSV)"  
- Fill in "File name": Select path from File Explorer (e.g., C:&#92;...&#92;Florida.csv)  
- "Okay" (might take a moment)  

#### To convert .csv to .mat:  

**Step 10:** Starting MATLAB  

- Start MATLAB  

**Step 11:** Converting .csv to .mat  

- "Import data"  
- Fill in "File name": Select path from File Explorer  
- "Open"  
- "Import selection" and close window  
- "Workspace" (lower LHS) > Right-click on table > "Rename"  
- Call "buildings" to keep generic  
- "Workspace" > Right-click on table > "Save as"  
- Fill in "File name": Select path from File Explorer (e.g., C:&#92;...&#92;Florida.mat)  
- Fill in "Save type as": "MAT-files (&ast;.mat)"  
- "Save"  

### Computing building-specific drag coefficients  

**Step 12:** Navigating to correct working directory  

- Download code: "city_texture_cd_model.mat", "run.mat", "index.m"  
- From "Current folder" (upper LHS), copy-paste your .mat files to the folder with "city_texture_cd_model.mat"  

**Step 13:** Running the code  

- In "Command window" (bottom of page), run "index.m"  

**Step 14:** Sharing the results  

- You'll have one .mat and one .csv file for drag coefficients in each state.  



## Instructions: loss_model  

To discuss the economic implications of texture, we need to:  

- Collect necessary information  
- Compute building-specific expected annual losses (EAL)  

### Collecting necessary information  

In one folder collect:  

- Your .mat files from **Step 14**  
- Files in ACS_DP03_mat.zip  
- Files in ACS_DP04_mat.zip  
- Files in HAZUS_mat.zip  
- Files in Neighbors_mat.zip  
- Files in PSAM_mat.zip  
- Entire code under **loss_model**  

### Computing building-specific expected annual losses (EAL)  

**Step 15:** Running the code  

- In "Command window" (bottom of page), run "index.m"  

**Step 16:** Sharing the results  

- You'll have 6 .mat and 6 .csv files for EALs in each state.  
- 2 of each are 'EAL' (in $1000 per household).  
- 2 are 'EAL_PerValue' (normalized by home value).  
- 2 are 'EAL_PerInc' (normalized by household income).  
- Each includes 'Case0' (for an unmitigated building stock) and 'Case1' (for a mitigated building stock).  
- Also, each includes 'Hazus' (based on a framework developed by FEMA) and 'New' (based on an updated framework).  

Additional reading on HAZUS:  

- On its terrain and wind modeling: [here](https://ascelibrary.org/doi/full/10.1061/%28ASCE%291527-6988%282006%297%3A2%2882%29)  
- On its loss modeling: [here](https://ascelibrary.org/doi/full/10.1061/%28ASCE%291527-6988%282006%297%3A2%2894%29)  
