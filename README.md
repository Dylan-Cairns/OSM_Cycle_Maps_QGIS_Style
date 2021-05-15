# OSM Cycle Map style QGIS stylesheets

This is stylesheets and instructions on how to use OSM data (courtesy of Geofabrik) to create an outdoor hiking map similar in style to OSM Cycle Map with the following features:

- Contours
- Hillshading
- Hiking trails
- Shelters 
- Additionally, hospitals and toilets are also shown, but you can change this.

Example:

OSM Cycle map:

![Screenshot 1](/screenshots/OSMCycle.png)

Screenshot of PDF created using these stylesheets:

![Screenshot 2](/screenshots/Mymap.png)

<sub>Hillshades <a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a></sub>

The stylesheets are based on [this](https://github.com/charleyglynn/OSM-Shapefile-QGIS-stylesheets) project. They are licensed under [CC-BY-SA](http://creativecommons.org/licenses/by-sa/3.0/) so you are free to use these stylesheets under the same terms.

Here are instructions on how to create an OSM Cycle Map style map from scratch in QGIS:

### 1. Create a project and basemap

- Create a new project in QGIS and ensure the CRS is set to EPGRS:4326 (bottom right hand corner of QGIS screen)
- Change the project background to a nice color. I like #f3f3f3 (project -> properties -> general -> background)

### 2. Download and add data to the project from Geofabrik

- Create a folder somewhere to hold all of your mapping data
- Get the OSM data from https://download.geofabrik.de/ and save it to a folder you created before (if you are mapping China, [this](https://download.geofabrik.de/asia/china-latest-free.shp.zip) is the file you need.)
- In QGIS, go back to your project and from the menu select layer -> add layer -> add vector layer -> select the file downloaded from Geofabrik, and then click add
- You will get a list of available layers to add. You only need these layers:
  - roads
  - places
  - natural_free
  - waterways_free
  - water_a_free
  - pois_free
  - pois_a_free
  - landuse_a_free

### 3. Style the Geofabrik data

- Download all of the QML files from this website and place them in the folder you created earlier.
- For each layer which is now in the map, right click on the map, select properties, then at the bottom of the window select style -> load style -> click ... and browse to the stylesheet with the same name as the layer.
- NOTE: if you try and add the stylesheet for pois_a_free you will get an error. That's because we need to process that layer first. (Geofabrik folks saved shelter info in this area layer, we have to convert it to points)
- Click on the pois_a_free layer. Now from the top menu select vector -> Geometry tools -> centroids -> run -> close. You will now have a new layer named centroids. Click on the small icon that appears in the menu next to the layer, and select make layer permanent. choose a filename and save the file in the same folder as the rest of the map data.
- Now you can apply the stylesheet for the pois_a_free layer to the newly created 'centroids' layer. You can also delete the original pois_a_free layer, we are done with it.

### 4. Add hillshading

While it's possible to create hillshades from SRTM data directly, the process seems to be complex and I haven't had much luck creating nice looking hillshades. If anyone can provide simple instructions on how to create attractive hillshading from SRTM data from directly within QGIS, please let me know.

In the mean time, the best source I've found is the hillshading layer from the maptiler topo style.

**NOTE: contact maptiler to inquire regarding copyright, attribution, or relevant fees.**

- Install the maptiler plugin in QGIS
- create an account at maptiler.com
- Navigate to the maptiler topo style: https://cloud.maptiler.com/maps/topo/
- Copy the web address which is in the box just after "Copy link to JSON style into OpenLayers, Mapbox GL JS or mobile SDKs."
- Back in QGIS, right click on the maptiler plugin in the left side menu and click 'add new map'. name it topo and paste the copied address where it says "JSON URL"
- 3 layers will be added to your map. only keep the hillshade layer - disable or delete the other two.

### 5. Add a print template, then save this project as a template for future projects

I have included a print template for printing A2 maps. Here's how to add it to your project:

- Select -> project -> layout manager -> go to 'new from template' and change 'Empty Layout' to 'Specific' and navigate to the print layout you downloaded from this site.

You are almost done!

Go to project -> 'save as' and save this project as a template. You will use this project as the base for each new map you create.

### 6. Add contour lines

This is the last step because this step needs to be done for each map project individually.

- Open the template you created earlier, and save a copy to be the base of this new map project.
- Zoom to the area where you want to make a map of. Make sure anything you want to be included in the map is on the screen
- Go to the plugin manager and install 'SRTM downloader'
- After downloading, go to the top menu and plugins -> SRTM downloader -> click the button 'Set canvas extent' -> click download
- You'll have to go to the weblink shown and setup an account
- Once the image downloads, it will appear as a black and white layer in your map.
- Click on the new layer, then from the top menu select Raster -> Extraction -> Contour
- In the menu, accept the defaults and click run.
- Once it's finished, apply the contours stylesheet which was downloaded from this website to the new contours layer.
- Save the new contours layer to a file, just like we did with the centroids layer earlier.
- You now have contour lines!

### Bonus info for Newbs trying to make hiking maps

**Adding GPX tracks to the map**

- From the top menu select vector -> gps tools -> browse to the GPX or KML file that has your tracks or waypoints

**Adding custom points or place names to maps**

There are built in ways to add points directly in QGIS but I found them overly complicated. I add custom place labels by creating waypoints in a GPX file in my preferred GPX editor [Routeconverter](https://www.routeconverter.com/home/en) and then add the GPX file to the map. 

**Adding Chinese + Pinyin labels from Maptiler**

The data in Geofabrik only has the default language. If you're making maps for China, that's Chinese. If you want to add pinyin subtitles:

- add the maptiler topo map same as before, but don't get rid of the 'Maptiler planet' layer. Download and apply the maptiler stylesheet from this website.
- open the 'roads' layer, go to the properties -> labels, and disable the labels.
- NOTE: The maptiler QGIS plugin is still a work in progress, and it seems it's not possible to control the size of the font on the labels. Luckily the sizing is reasonable as is. I can't seem to get road names to show up though.
- From the settings on maptiler it seems getting Chinese + English labels is an option, but the language display formatting is wierd and you'll have to play with the matching rules and create a new stylesheet.

### To do
All of the place names (Village, Town, City, etc) are currently the same size. That doesn't bother me, but you can add rule based labelling if you like:
Select the places layer, properties -> labels, change the bar 'single labels' to 'rule based labelling' and create a new rule for each type of place, using "fclass" = 'Village' and so on for the labelling rules. You can refer to the [Geofabrik documentation](https://download.geofabrik.de/osm-data-in-gis-formats-free.pdf) as needed.

Happy mapping!
