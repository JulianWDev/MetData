---
layout: post
title: Assignment 1 - Water
author: Carlotta, Camille, Lora, and Julian
---

When considering data sources available on water quality and traffic in Amsterdam it's hard to find holistic and detailed data sets online. However, for this assignment we have considered a combination potential data sets available and other sources for information that could be combined to create a somewhat complete image, orat least a sufficient image to pick a route. We will first discuss potential datasets on marine traffic and then go over the possibilities for water quality, or quality approximations in Amsterdam. 

### Datasets for marine traffic 

When considering the need to not interfere with water transport we haven’t found much accesible raw data which can be used in python. However, we have found a pdf map which shows no go zones with a red sign and restricted areas for boat transport in pink (Sloepdelen.nl, n.d.). The IJ is restricted likely due to the passage of large freigh traffic so we want to avoide that, but other restricted areas might actually be interesting for us since they will have less boat traffic. However, we cannot plot this map since it comes in a pdf format.

![boat_traffic](./information_amsterdam_en.pdf)

Similarly, the platform marine traffic we can see a live feed of the main traffic all over the world (MarineTraffic, n.d. ). This also showes where boats move through amsterdam currently and over time. While this data is not available to us directly, through a scraper tool one could over time collect the data and make a detailed map of which canals are used and how much.This would liekly provide the most accrate data in the future however ot fot his porjetc. 

For this assignment we looced for data we could directly use for coding, even if that might mean some level of compromise in accuracy. Thus, we used a route ste up on google maps which showed four main tourists routes to take when in Amsterdam. We assume that these four routes will likely cover the canals in amsterdam which are the most frequented for leisure traffic and thus where the race would have the most impact [Google maps link.](https://www.google.com/maps/d/viewer?mid=1-isVe-eoiAiJj18lnT3cmeoLnbw&hl=en_US&ll=52.375187000000004%2C4.8828430000000145&z=15 ). This data is a geometric shapefile and originally comes as XML file, which we then converted to a GEOJSON file. Its a combination of lines and nodes, but visualized as an object later in thsi document.

### Datasets for water quality  

However, to consider feasibility of the event the main focus point is water quality. There are several different monitoring points. Primarily we found specific data about water quality as mentioned on websites thus hard to pull together as a holistic data set (Gemeente Amsterdam, 2023). Similarly, Waternet has a map which shows the oxygen and salt level in the canals of Amsterdam (Van Roekel, 2022).  Again, this map is not downloadable and thus the information would have to be manually entered. Thus, here some scraping tool might be useful.  

 

In research we found that the E. Coli bacteria mainly determines whether water quality is good enough, but we didn't find datasets on E.Coli concentrations (Van den Tillaart, 2022). For the sake of this exercise, we have focused on data sets which are downloadable and while they might not give a perfect image of areas in Amsterdam with clean water. Clean water areas in combination with the water quality of the ground water in certain other areas might give some idea of the better swimming locations. 

  

Our primary starting point for water quality is the map of possible swimming locations in Amsterdam (Gemeente Amsterdam, n.d.). This data set can be downloaded in several formats but predominantly csv and GEOjson. Both are easily legible also without a program as they can be opened in Excel and an online browser. However, because this information is geographic in nature, for later processing the geographic format GEOjson is easier to use in Python. The file has point data but is set up as a standard number file. The file formats are both quite straightforward to open and geopandas or pandas are some of the most straightforward libraries in python that can probably read the data. For analysis plotly might be the best option to graph but for geographic analysis one should stick with geopandas. In addition, the mapbox plug-in with plotly or omnx data from OpenStreetMap, could be used to situate the points on a map. 

Overall, the file contains the name of the place, categories of the type of swim water (pool, official outdoor spot, etc), an ID code and the coordinates of the location. It is important to consider that this data set contains both indoor and outdoor locations which are safe for swimming. Thus, within these categories one should filter for outdoor locations and the canal. Furthermore, the file is semicolon delimited and has commas, which should be considered when reading it with pandas. 


### Decision on best route

We plotted the possible swimming locations and the busiest canal boats traffic routes in a map of Amsterdam (Amsterdam Canal Routes - Google my maps, n.d. ; Gemeente Amsterdam, n.d.). Many of the outdoor swimming spots are far out of the city center which is inconvenient if we consider accessibility and the location of hotels and airbnbs. Since we know that there is good swimming water next to AMS and the city swim was also located there, we think that is the best route (Amsterdam City Swim, n.d.). For assignment 4, we decide to use AMS as a starting point and will create the route based on the canal traffic.

### References 

Amsterdam Canal routes - Google my maps. (s. d.). Google My Maps. https://www.google.com/maps/d/viewer?mid=1-isVe-eoiAiJj18lnT3cmeoLnbw&hl=en_US&ll=52.375187000000004%2C4.8828430000000145&z=15 

Amsterdam City Swim. (n.d.). Locatie and Route. Retrieved October 10, 2023, from https://www.amsterdamcityswim.nl/informatie/locatie-en-route

Gemeente Amsterdam. (n.d.). Zwem- en speelwater. https://maps.amsterdam.nl/zwemwater/ 

Gemeente Amsterdam. (2023). Waterkwaliteit Sloterstrand. Retrieved October 10, 2023, from  https://www.amsterdam.nl/toerisme-vrije-tijd/parken/sloterpark/waterkwaliteit-sloterplas/ 

MarineTraffic. (n.d.).  https://www.marinetraffic.com/en/ais/home/centerx:4.910/centery:52.375/zoom:1 

Sloepdelen.nl. (n.d.). Sailing rules on the water. Retrieved, October 10, 2023 from https://sloepdelen.nl/downloads/information_amsterdam_en.pdf 

Van Roekel, A. (2022, April 29). Bekijk: Koel, helder grachtenwater. NEMOKennislink. https://www.nemokennislink.nl/publicaties/koel-helder-grachtenwater/ 

Van den Tillaart, A. (2017, February). (Swim) water quality modelling in the city of Amsterdam. [Master's thesis, Wageningen University.] 