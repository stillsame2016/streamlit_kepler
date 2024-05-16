# WENOKN Streamlit Kepler Component

Kepler.gl is a powerful open-source geospatial visualization tool developed by Uber. It enables users to effortlessly
create custom, interactive maps from large-scale location data. With its intuitive interface and extensive customization
options, Kepler.gl empowers users to explore and analyze geographic patterns, trends, and relationships. Whether for
urban planning, logistics optimization, or data journalism, Kepler.gl provides a user-friendly solution for visualizing
complex spatial datasets. Its versatility, ease of use, and ability to handle big data make it an essential tool for
anyone needing to understand and communicate insights from geospatial information effectively.

The following three libraries - geemap, leafmap, and streamlit-kepler - currently offer support for integrating
Kepler.gl into Streamlit, facilitating data presentation. However, they are limited to static Kepler maps. "Static"
implies that once the map is rendered, Streamlit cannot manipulate it further or retrieve its current state. To modify
data, the map must be re-rendered. This restriction significantly constrains Kepler's utility in Streamlit. Many
applications demand dynamic capabilities, enabling Streamlit and Kepler to communicate bidirectionally - adding or
removing data at any time and responding to user interactions on the map promptly and flexibly.

We've created a Streamlit component for Kepler, enabling seamless bidirectional communication between Streamlit and
Kepler. Now, Streamlit can effortlessly retrieve the map's current state and dynamically adjust the application
accordingly. Additionally, Streamlit gains the ability to add data to the map on-the-fly, independent of whether the map
has been previously rendered. This breakthrough empowers users to interact fluidly with the map, enhancing the
application's versatility and responsiveness.

## Installation instructions

```sh
pip install streamlit-kepler-component
```

## Usage instructions

In the present prototype, Kepler.gl accepts the following parameters:
* A list of GeoDataframes or Dataframes
* options
* config
* height

When there's an interaction with the map, such as a mouse movement, the Kepler streamlit component returns the current map configuration. In simpler terms, any action performed on the map will prompt the entire page to be re-rendered. To ensure smooth functioning and avoid potential bugs, it's essential to utilize session state management for your web page and execute any prolonged actions within a separate thread.

## Example

```
import json
import streamlit as st
import geopandas as gpd
from keplergl import keplergl

if "datasets" not in st.session_state:
    st.session_state.datasets = []

sf_zip_geo_gdf = gpd.read_file("sf_zip_geo.geojson")
sf_zip_geo_gdf.label = "SF Zip Geo"
sf_zip_geo_gdf.id = "sf-zip-geo"
st.session_state.datasets.append(sf_zip_geo_gdf)

h3_hex_id_df = pd.read_csv("keplergl/h3_data.csv")
h3_hex_id_df.label = "H3 Hexagons V2"
h3_hex_id_df.id = "h3-hex-id"
st.session_state.datasets.append(h3_hex_id_df)

map_config = keplergl(st.session_state.datasets, height=400)

```


