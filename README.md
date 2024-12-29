# SeismicData
# 📊 **Seismic Data Visualization Project**

**A project for analyzing, visualizing, and mapping seismic data using MiniSEED files and IRIS API data.**

---

## 🚀 **Project Overview**

This project processes and visualizes seismic data from MiniSEED files and IRIS API. It includes:
- Importing and storing seismic data into a **SQLite database**.
- Creating **Helicorder-style charts** from seismic data.
- Interactive **Folium-based maps** with station markers.
- Data filtering by **network** and **MiniSEED availability**.
- Search and filter capabilities with interactive map controls.

---

## 📦 **Features**

- ✅ **Data Import:** Supports importing `.mseed` files into a database.
- ✅ **Interactive Maps:** View station locations with dynamic clustering.
- ✅ **Helicorder Charts:** Visualize seismic activity with time-series charts.
- ✅ **Search & Filter:** Filter stations by network and data availability.
- ✅ **Dynamic Popups:** Metadata and charts displayed on station markers.

---

## 🛠️ **Setup Instructions**
### **Folder setup**
- ├── main.ipynb            # Google Colab Notebook
- ├── README.md             # Project Documentation
- ├── SEP/                 # Seismic data files (.mseed) for SEP, generic import can pull any .mseed format
- └── Data/database.db           # SQLite Database (ignored by Git)
- └── Test_Data/*.db&.mseed      # SQLite testing DB and single test mseed
### **1. Start Here**

Run the following setup block in your **Google Colab Notebook** Or Locally,
you will possibly need to run this section twice:

```python
# Start Here
# Install necessary libraries from https://docs.obspy.org/packages/obspy.io.mseed.html
import os
# This sets up the environment with the correct packages and restarts the Colab runtime. 
# You will need to run this section twice
if not os.path.exists("restart_flag.txt"):
    # Install Required Packages
    %pip install obspy pandas matplotlib folium sqlalchemy

    # Create a restart flag file
    with open("restart_flag.txt", "w") as f:
        f.write("restart")

    # Restart the runtime
    print("✅ Packages installed. Restarting runtime to apply changes...")
    os.kill(os.getpid(), 9)
else:
    # Remove the restart flag file
    os.remove("restart_flag.txt")

    # Continue with the rest of your code
    print("✅ Runtime restarted. Continuing with the next steps...")

    # Import packages after installation
    import obspy
    import pandas as pd
    import matplotlib
    import folium
    from folium import Element, LayerControl, FeatureGroup
    import sqlalchemy
    import sqlite3
    import io
    import logging
    import gc
    import requests
    import unittest
    import matplotlib.pyplot as plt
    from folium.plugins import MarkerCluster, Search, TagFilterButton
    import base64
    from IPython.display import display
    from obspy import read

    print("✅ All packages successfully installed and imported.")
```
### **2. Usage**
Define the SQLite DB location, default = 'Data' in main():
```python
def main():
    #Initialize Database
    db_file_path = 'Data'
    ensure_directory_exists(db_file_path)
    conn = get_connection(db_file_path)
    cursor = conn.cursor()
    cursor.close()
    conn.close()
    #Creates a SQLite DB in the given file path, Default 'Data/'
    conn = get_connection(db_file_path)
    cursor = conn.cursor()
    #Creates the tables
    create_tables(cursor)
    #...
```
Define the mseed folders to import in main():
```python
def main():
    #...
    #imports the mseed files in the given path, default 'SEP/'
    mseed_file_path = 'SEP/'
    ensure_directory_exists(mseed_file_path)
    import_streams(conn,cursor,mseed_file_path)
    conn.commit()
    #Queries the IRIS API for the location data, and updates the station table
    update_stations(conn,cursor)
    conn.commit()
```
Define a network filter if needed in main():    
```python
def main():
    #...
    #Imports stations using the IRIS API, the second arguement is used to filter
    # based off of the given network string,if you don't give a second arguement it will
    # import all the stations.
    #import_stations_api(conn,cursor, "CC")
    #import_stations_api(conn,cursor)
    network_filter = "CC"
```
### **3 Unit testing the notebook**
Unit testing will require you to the create a Test_Data folder, and a single mseed file within it.
I advise you to comment out main() when doing unit testing
```python
if __name__ == '__main__':
    unittest.main(argv=[''], exit=False)
    #main()
```
### **4. Run the notebook**
Run the setup, configure in main(), comment out testing if not needed, and then activate:
```python
if __name__ == '__main__':
    #unittest.main(argv=[''], exit=False)
    main()
```
### **5. Using the visualizations**
Using the legend, you can click into clusters that have data:
![alt text](legend.png)
Here's an example of the map without any filter for networks:
![alt text](All_stations.png)
As you click into the clusters you will drill down into markers that have data:
![alt text](Data_markers.png)
Clicking one with provide the user with a helicorder graph, and mseed metadata:
![alt text](Graph_metadata.png)
To reduce noise on the map, you can filter markers with or without data:
![alt text](Filter.png)
To find specific stations you can search by station name or network, 
click the item in the search box, and then a red circle will appear around it
![alt text](Search_1.png)
![alt text](Search_2.png)
Searching for HOA Station
![alt text](Search_3.png)
Searching for SUG Station
![alt text](Search_4.png)
### **6. Future Additions**
- 1: Add parallel processing for the imports, api calls, and generation of the map
- 2: Add generalization and static objects for the javascript, images, and marks used in the html

## 📧 Contact
For inquiries, please reach out via:
- 📧 **Email:** [timothy.g.jones2@gmail.com](mailto:timothy.g.jones2@gmail.com)  
- 🔗 **LinkedIn:** [linkedin.com/in/timothy-jones-735417265](https://linkedin.com/in/timothy-jones-735417265)
