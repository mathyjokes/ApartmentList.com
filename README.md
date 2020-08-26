# Am I overpaying for my apartment?
An analysis of Washington, DC apartment rentals from ApartmentList.com. 
This analysis includes
  - initial scrape of DC apartments from ApartmentList.com
  - geocoding addresses to get exact coordinates
  - clean and restructure apartment list to include distance from center city, number of rooms, etc
  - visualize data and conduct initial exploration
  - create random forest model to predict rental prices
  
There are many factors that go into apartment rental prices in DC. The features collected from ApartmentList.com include
  - Address
  - Description (containing general building description)
  - Units Available (indicating desirability of specific apartment building)
  - Apartment Type (Studio, 1 Bedroom, etc)
  - Square Footage
  
After a little bit of cleaning and restructuring, these apartments can be plotted onto a heatmap
![apt_heatmap](https://github.com/mathyjokes/ApartmentList.com/blob/master/apartment_heatmap.png)

Our question is not about apartment location, but rather about apartment prices.
Once we build a random forest model, we can look at which features are most important for price prediction.
![feature_importance](https://github.com/mathyjokes/ApartmentList.com/blob/master/feature_importance.png)

Looks like apartment footage has the most important effect on apartment rental price prediction in DC. 
Proximity to the White House and to the Capitol building, variables added to indicate neighborhood desirability, have the smallest effect.

When we look at the rental price vs the square foot of apartments, we can see this effect intuitively
Note that an outlier was removed from this data - an 8 bedroom apartment available in the Georgetown area, which is one fo the most expensive in the city.
![sqft_price](https://github.com/mathyjokes/ApartmentList.com/blob/master/sqft_vs_rentalprice_no_outliers.png)

This whole excercise had a clear purpose: in the end, I want to know if I'm overpaying for my apartment.
So, I ran the model against my own apartment information.

The model predicts I should be paying ~$200 more than I do -- looks like I got a great deal! Guess I'll stick around a bit longer.
