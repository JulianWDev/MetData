---
layout: post
title: Assignment 3 - Housing
author: Carlotta, Camille, Lora, and Julian
---

### What Amsterdam will receive from tourist tax if the event lasts a week and you will have 30.000 visitors?
We can calculate the tourist tax with a simple calculation using the airbnb data. (City of Amsterdam, n.d.a).
```python
# In amsterdam the tax is 3 euros and we assume a stay of 6 night since the arrival and depature day would be included in the 7
tourist_tax = 3 * 30000 * 6

# Furthermore, there is a tax of 7% on the price of the room
# To calculate this, we first calculate the average price of an airbnb per night
airbnb_avg_price = np.average(airbnb_df['price'])

# If we consider that, on average, 2 people will stay per AirBNB, we can calculate the total tax
tax_room = 0.07 * airbnb_avg_price * 15000 * 6

# So if all 30 000 visitors would stay in an AirBNB, the total tax would be:
print("The total tourist tax earned by Amsterdam from the 30.000 visitors would be: " + str(np.round(tourist_tax + tax_room)) + "€")
```
`The total tourist tax earned by Amsterdam from the 30.000 visitors would be: 2143286.0€`

### Plot the amount of AirBnB locations per neighbourhood.

![Alt text](airbnb.png)

The map above shows an overview of all the aibnbs in Amsterdam and their location in Amsterdam.

We can plot the amount of AirBNB location by neighbourhood using the following code (Gemeente Amsterdam, 2023; Inside Airbnb, n.d.):
```python
airbnb_df = pd.read_csv('./listings.csv')
bbga_df = pd.read_csv('./bbga.csv', sep=';')
df_neighbourhood = airbnb_df['neighbourhood'].value_counts().reset_index().rename(columns={'index': 'neighbourhood', 0 : 'count'})
figure = px.bar(df_neighbourhood, x='neighbourhood', y='count')
figure.show()
```
<iframe src="./embeds/airbnb_distribution.html" height="300" width="100%" style="border:none"></iframe>
First, we read the airbnb dataset which we downloaded from the airbnb scraper website. Then, we use `value_counts` to create a new data frame that contains the count of each occurrence of a specific neighbourhood. We plot this new data frame in a histogram using plotly express.


### Which street in Amsterdam has the most AirBnB apartments?
We need to find the street address for each of the AirBNB listings in the database. To do this, we first put all of the coordinates of the listings into a dataframe.
```python
import reverse_geocoder as rg
from geopy.geocoders import Nominatim
airbnb_df["coord"] = airbnb_df.latitude.astype(str) + ", " + airbnb_df.longitude.astype(str)
coordinates = airbnb_df["coord"].tolist()
result = []
```
Then, we use Nominatim to find the addresses for each of the coordinate points in the data frame. We format the resulting data and then add it to a list with all of the addresses.
```python
geolocator = Nominatim(user_agent="#####")
for i in coordinates:
    address = geolocator.reverse(i)
    split_data = address.address.split(',')
    short_data = split_data[:7]
    result.append(short_data)
```
However, the formatting for each of the list items is not machine-readable. To be able to parse this information, we use a visual inspection to conclude that the street name is in one of the first three columns. So we then create a new list which only includes the variables within the first 3 columns, then we count how many times each individual item show-up. From here we print the top of the value count list and scan for the first item which is a street name and and how often it appears.

```python
adresses_rel = pd.concat([adresses['1'], adresses['2'], adresses['3']])
count = adresses_rel.value_counts()
```
`The street Nassaukade has the most AirBNB's, with 183 registered listings. `

### Try to cross reference the data from the AirBnB dataset with the BBGA. Can you figure out if all apartments of AirBnB are designated as housing? Which number of apartments are not rented out all the time but are also used as normal housing?
First, we filter the BBGA and select only the rows with relevant data:
```python
bbga_airbnb_df = bbga_df[['gebiednaam', 'sdnaam', 'BHVESTAIRBNB']]
bbga_airbnb_df_GWW = bbga_airbnb_df.iloc[3937:3962]
bbga_airbnb_df_GWW.to_csv("bbga_short.csv", index = False)
```
Then, we need to rename the rows in the BBGA file to be the same as those in the airbnb file.
```python
bbga_short_df = pd.read_csv('./bbga_short.csv')
bbga_short_df.replace({ "Oud West, De Baarsjes" :"De Baarsjes - Oud-West",
            "De Pijp, Rivierenbuurt":"De Pijp - Rivierenbuurt",
            "Indische Buurt, Oostelijk Havengebied" : "Oostelijk Havengebied - Indische Buurt",
            "IJburg, Zeeburgereiland" : "IJburg - Zeeburgereiland",
            "Geuzenveld, Slotermeer" : "Geuzenveld - Slotermeer",
            "Buitenveldert, Zuidas" : "Buitenveldert - Zuidas",
            "Oud-Zuid" : "Zuid" ,
            "De Aker, Sloten, Nieuw-Sloten" : "De Aker - Nieuw Sloten",
             "Gaasperdam" : "Gaasperdam - Driemond"}, inplace = True)
```
Next, we plot the two files together in a bar graph using plotly express.
```python
graph = px.bar(bbga_short_df, x='gebiednaam', y='BHVESTAIRBNB', barmode='group', title="Comparison of AirBnBs in Amsterdam per data set", text_auto=True)
graph.add_bar(x=df_neighbourhood['neighbourhood'], y=df_neighbourhood['count'], name="AirBNB dataset", text=df_neighbourhood['count'])

graph.show()
```
<iframe src="./embeds/comparison.html" height="400" width="100%" style="border:none"></iframe>
The BBGA dataset is in blue, the airbnb dataset is in red.

Presumably the BBGA comes out so much higher than the airbnb listings, since it collects data throughout the year and showes the sum from the year. The Airbnb data is a snapshot of the site at the moment of time we download the data. Consequently, it will leave out listings which might be registered in the BBGA because they are no longer online.

To see whether or not an airbnb is also used as a house we consider the variable availabilit_365. This variable presumably shows how many days a place is available on airbnb and thus to what extent it is also used for other means.
```python
airbnb_df.hist(column='availability_365', bins=[0, 5, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200, 210, 220, 230, 240, 250, 260, 270, 280, 300, 310, 320, 330, 340, 350, 360, 370])

# Set the title and axis labels
plt.title('Days Airbnb is available')
plt.xlabel('number of days')
plt.ylabel('amount of airbnbs')
```
![Availability histogram](./airbnb_availability_histo.png)

From this, we can conclude that the vast majority of airbnb's are available only some days in a year, with a lot of them almost never available. 

```python
#from research we can see that private houses can only be rented out for 30 days maximum, thus we can assume
# that all houses which are online for less than 30 days each year are also used for housing
also_housing = airbnb_df[airbnb_df['availability_365'] <= 30]
print(len(also_housing))
```
`4586`

As seen in City of Amsterdam (n.d.b) private residences can only be rented out for a maximum of 30 days if they are zoned as housing. Thus we can assume that the listings which are available for less than 30 days a year are also used as housing, which means around 4586.


### How many hotel rooms should be built if Amsterdam wants to accommodate the same number of tourists?
We create a new list with only airbnbs which have been online for more than 0 nights so minimum 1 day, and then we count how many items are in our list to see how many airbnbs this counts for.
```python
# we create a new list with only airbnbs which have been online for more than 0 nights so minimum 1 day
# and then we aim to see how many beds these locations have
airbnb_more_0 = airbnb_df[airbnb_df['availability_365'] > 0]
print(len(airbnb_more_0))

#we split the name column into its components
airbnb_more_0[['house', 'rating', 'nr_bedrooms', 'nr_bed', 'bathrooms']] = airbnb_more_0['name'].str.split("·", expand = True)

# we extract the numerical value
airbnb_more_0['beds'] = airbnb_more_0['nr_bed'].str.extract(r'(\d+(?:\.\d+)?)').astype('float')
#we create a list of the numericla values
list_beds = airbnb_more_0['beds']

#we set up out empty variables
nr_bed_airbnb = 0
x=0

#we itterate through our list and add it to
for i in list_beds:
    try:
        adding = int(i)
        nr_bed_airbnb = nr_bed_airbnb + adding
    except:
        # allows me to see the number of exceptions
        x = x+1

#this i number of beds in airbnb
print(nr_bed_airbnb)
#standard hotel rooms have 2 beds which is the assumption for the next calculation
hotel_rooms = nr_bed_airbnb/2
print(hotel_rooms)
```
`5438
9413
4706.5`

From this calculation, we can conclude that, currently the Airbnbs in Amsterdam have around 9413 beds, and if we assume that each hotel room hosts 2 beds, we would need to build around 4707 extra hotel rooms.

### How many different licenses are issued?
If we remove duplicate licenses, the length of the list will be the amount of different licenses.
```python
license = list(set(airbnb_df["license"])) 
exempt_licence = airbnb_df[airbnb_df['license'] == "Exempt"]

print("The number of licences after removing duplicates :" + str(len(license)))
print(str(len(exempt_licence)) + " are exempt")
```
`The number of licences after removing duplicates :7289`

`609 are exempt`

In principle, every vacation rental needs its own personal license code which proves that they are legally allowed to rent out (Airbnb, n.d). This is reflected in the broad set of licenses we see initially. But what is really interesting is that we see 609 houses which are exempt from having a license which seems like a high number considering having a licence is obligatory.

### References

Airbnb. (n.d.) Registration in Amsterdam. https://www.airbnb.nl/d/registrationamsterdam?_set_bev_on_new_domain=1697731796_ZDJhMTg2ZDY3MWRl 

City of Amsterdam. (n.d.a). Tourist tax (toeristenbelasting). https://www.amsterdam.nl/en/municipal-taxes/tourist-tax-(toeristenbelasting)/

City of Amsterdam (n.d.b) Apply for a permit. https://www.amsterdam.nl/en/housing/holiday-rentals/applying-permit/ 

Gemeente Amsterdam. (2023, June 15). Data en informatie. https://data.amsterdam.nl/datasets/rl6-35tFAw2Ljw/basisbestand-gebieden-amsterdam-bbga/

Inside Airbnb. (n.d.). Get the Data. http://insideairbnb.com/get-the-data 