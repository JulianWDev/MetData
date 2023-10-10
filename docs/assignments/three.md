---
layout: post
title: Assignment 3 - Housing
author: Carlotta, Camille, Lora, and Julian
---

### What Amsterdam will receive from tourist tax if the event lasts a week and you will have 30.000 visitors?
We can calculate the tourist tax with a simple sum. 3 euros times 30.000 visitors times six nights.
```python
tourist_tax = 3 * 30000 * 6
print(tourist_tax, "euros")
```
`540000 euros`

### Plot the amount of AirBnB locations per neighbourhood.
We can plot the amount of AirBNB location by neighbourhood using the following code:
```python
airbnb_df = pd.read_csv('./listings.csv')
bbga_df = pd.read_csv('./bbga.csv', sep=';')
df_neighbourhood = airbnb_df['neighbourhood'].value_counts().reset_index().rename(columns={'index': 'neighbourhood', 0 : 'count'})
figure = px.bar(df_neighbourhood, x='neighbourhood', y='count')
figure.show()
```
![neighbourhood_plot](./neighbourhood_plot.png)
First, we read the airbnb dataset which we downloaded from the airbnb scraper website. Then, we use `value_counts` to create a new dataframe that contains the count of each occurance of a specific neighbourhood. We plot this new dataframe in a histogram using plotly express.

### Which street in Amsterdam has the most AirBnB apartments?
We were unable to complete this assignment, because it required reverse geocoding which is a paid service. Using google maps API, it would have costed about 40$. Free solutions were inadequate, because they disallow bulk API requests.

### Try to cross reference the data from the AirBnB dataset with the BBGA. Can you figure out if all apartments of AirBnB are designated as housing? Which number of apartments are not rented out all the time but are also used as normal housing?
First, we