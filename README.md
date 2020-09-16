# Am I overpaying for my apartment?
Like all tenants, I want to know if I am getting a fair deal on my rental apartment. After deciding it would be pointless to ask my landlord to assess if they are setting pair rental prices, I turned to the power of statistics to understand the Washington, DC apartment rental market. I was inspired in my search by the great article by Brunna Villar on predicting housing prices in Amsterdam (https://towardsdatascience.com/ai-and-real-state-renting-in-amsterdam-part-1-5fce18238dbc)!

There were a few steps to complete before plugging in my apartment information and receiving a price, however. My analysis includes:
  - An initial scrape of 2,443 apartment listings from ApartmentList.com
  - Geocoding addresses to get extact coordinates from Google
  - Cleaning and restructuring apartment listings to include added features like distance from the White House, number of rooms, etc
  - Visualizing the data and conducting some initial exploration to learn what is going on
  - Creting a random forest model to predict rental prices
  
After checking the robots.txt file of ApartmentList.com (of course!) I was ready to use BeautifulSoup to get my data on apartment rentals in the area.
  
There are many factors that go into apartment rental prices in DC. The features collected from ApartmentList.com include
  - Address
  - Description (containing general building description)
  - Units Available (indicating desirability of specific apartment building)
  - Apartment Type (Studio, 1 Bedroom, etc)
  - Square Footage
  
I wanted to add more information to each apartment, however. Knowing that the White House and the Capitol building are two major landmarks that differentiate parts of the city, I wanted to know how far each apartment was from these touchstones. First, that required assigning coordinates to the addresses using Google's Geocoding API. From there, calculating distance was a breeze.
  
After a little bit more cleaning and restructuring, these apartments can be plotted onto a heatmap

![apt_heatmap](https://github.com/mathyjokes/ApartmentList.com/blob/master/apartment_heatmap.png)

Our question is not about apartment location, but rather about apartment prices. So onto statistics we go!

We build a random forest model to take in all of the features we've scraped or created, and spit out a possible rental price. After some hyperparameter tuning, we are ready to call the model our own. From this, we can look at which features are most important for price prediction.

![feature_importance](https://github.com/mathyjokes/ApartmentList.com/blob/master/feature_importance.png)

Looks like apartment footage has the most important effect on apartment rental price prediction in DC. 
Proximity to the White House and to the Capitol building, variables added to indicate a measure of neighborhood desirability, have some of the smallest effects.

When we look at the rental price vs the square foot of apartments, we can see this effect intuitively.
Note that an outlier was removed from this data - an 8 bedroom apartment available in the Georgetown area, which is one fo the most expensive in the city.

![sqft_price](https://github.com/mathyjokes/ApartmentList.com/blob/master/sqft_vs_rentalprice_no_outliers.png)

This whole excercise had a clear purpose: in the end, I want to know if I'm overpaying for my apartment.
So, I ran the model against my own apartment information.

The model predicts I should be paying ~$200 more than I do -- looks like I got a great deal! Guess I'll stick around a bit longer.
