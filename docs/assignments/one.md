---
layout: post
title: Assignment 1 - Water
author: Carlotta, Camille, Lora, and Julian
---

When considering data sources available on water quality and traffic in Amsterdam it's hard to find holistic and detailed data sets online. However, for this assignment we have considered a combination potential data sets available and other sources for information that could be combined to a more complete image. We will first discuss datasets on marine traffic and then go over the possibilities for water quality in Amsterdam. 

### Datasets for marine traffic 

When considering the need to not interfere with water transport we haven’t found easily accessible raw data. However, we have found a pdf map which shows no go zones in pink (Sloepdelen.nl, n.d.). We can’t subtract specific data from this map. Furthermore, via the marine traffic website we can find the main traffic routes all over the world (MarineTraffic, n.d. ). This includes frequently used routes in Amsterdam. Through a scraper tool one could also over time derive all of the data through time which would allow a more detail consideration for the event managers to consider. Neither of these data sources are directly usable in python in their current format. 

Concerning the canals routes, a google maps user made a list of the four canal routes someone should take if they are about to visit Amsterdam (Amsterdam Canal Routes - Google my maps, n.d.). This list recommends tourists what routes they should take if they are renting a boat for the day. It is fair to assume that touristic canals use the same routes. As the list is made on google maps, it is possible to extract these data and import them on python. The file type is kml, but we found an online converter that could create a geojson file out of a kml file.

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