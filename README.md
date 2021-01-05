# LA-County-PurpleAir

Jupyter notebooks that use the PurpleAir "experimental" API to map LA County census tracts to PurpleAir sensor data.

## **LA County PuprleAir Sensors.ipynb**
The main notebook. Collects PurpleAir sensor data, and for each census tract in LA County, calculates a weighted average PM2.5 value using the nearest sensors to that census tract. Finally, the data is saved as a geojson.

- ### sensor_data()
    Function that uses the "experimental" PurpleAir API. Unfortunately, the API is very barebones and can only return a JSON of all global sensors. This data is filtered to remove sensors with missing data, data that is more than 12 hours old, outlying sensors, and sensors with missing data. 

    **PurpleAir JSON descriptors**

    - pm = current PM2.5 reading
    - pm1 = raw PM1 reading
    - pm_10 = raw PM10 reading
    - pm_0 = current PM2.5 reading
    - pm_1 = 10 minute PM2.5 average
    - pm_2 = 30 minute PM2.5 average
    - pm_3 = 1 hour PM2.5 average
    - pm_4 = 6 hour PM2.5 average
    - pm_5 = 24 hour PM2.5 average
    - pm_6 = One week PM2.5 average
    - p1 = Particles >= 0.3 µm
    - p2 = Particles >= 0.5 µm
    - p3 = Particles >= 1.0 µm
    - p4 = Particles >= 2.5 µm
    - p5 = Particles >= 5.0 µm
    - p6 = Particles >= 10.0 µm
    - flags = Data flagged for unusually high readings
    - age = Sensor data age (when data was last received) in minutes
    - isOwner = Currently logged in user is the sensor owner
    - Adc = The voltage reading on the analog input of the control board
- ### los_angeles_county_sensors()
    Function that returns a dataframe of active PurpleAir sensors located within LA County. Sensor location data uses lat/lon. Function can use either a rough bounding box around LA County or a precise arcgis geojson of the geometries of LA County. Sensors in the dataframe are iterated through to find those that are within LA County. 
    - If the shape is used, a Point object is built from each sensor's Lon and Lat values, and then inserted into an rtree index. The rtree is used to efficiently find overlapping geometries.
    - If the bounding box is used, only those sensors with Lons and Lats within the bounding box values are used.
- ### la_county_census_tracts()
    Function that returns a dataframe of census tracts within LA County. The dataframe includes the census tract geometry as well as the geometry coordinates.
- ### find_nearest_sensors()
    Function that returns a dataframe that includes the n number of nearest sensors for each census tract in LA county. It uses rtree index's "nearest" function to find the indexed point closest to the bounding box of the census tract geometry. 
- ### census_tract_values()
    Function that calculates a weighted average using the inverse distance for the n number of nearest sensors' PM2.5 values. Returns the census tract dataframe with the average value included.
    
## **Plot LA County PurpleAir Sensors.ipynb**
Notebook for plotting using geopandas to read the data.geojson file.
