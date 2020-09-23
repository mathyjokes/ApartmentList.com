# Am I Overpaying for my Apartment?
Like all tenants, I want to know if I am getting a fair deal on my apartment.

Like all data scientists, I want to use any data I can get my hands on to answer my questions.

Luckily for me, I can combine curiosity with a focus on data by considering apartment rental rates from ApartmentList.com. By scraping, structuring, and modeling this apartment information, I can determine the predicted value of my apartment - and see if I'm being asked to pay too much.

Before getting started, I'd like to thank Brunna Villar for their great article on predicting housing prices in Amsterdam (https://towardsdatascience.com/ai-and-real-state-renting-in-amsterdam-part-1-5fce18238dbc), which first sent me down this rabbit hole.


### So, how do you even start?
To understand if I am getting a fair deal on my apartment, I first have to understand the apartment rental market in Washington, DC, where I live. What I want to do is tell the machine a few specs about my apartment and have it tell me what I *should* be paying.

There were a few steps to complete before plugging in my apartment information and receiving a price, however. My analysis includes:
  1) An initial scrape of 2,443 apartment listings from ApartmentList.com
  2) Geocoding addresses using Google's API to get extact coordinates
  3) Cleaning, restructuring, and supplementing apartment listings to include added features like distance from the White House, number of rooms, etc
  4) Visualizing the data and conducting some initial exploration to learn more about the data limitations
  5) Extracting parts of speech from building descriptions to understand key amenities
  6) Creating a random forest model to predict rental prices
  
### Ok, let's drop in

#### 1) Initial Scrape
The first step is to actually get the data. After checking the robots.txt file of ApartmentList.com (of course!) I was ready to use BeautifulSoup to get my information on apartment rentals in the area. The features collected from ApartmentList.com include:
  - Address
  - Description
  - Units Available
  - Apartment Type (Studio, 1 Bedroom, etc)
  - Square Footage

#### 2) Geocoding
Each of these features can tell us interesting information about the apartment, but I wanted more. Knowing that the White House and the Capitol building are two major landmarks that differentiate parts of the city, I wanted to know how far each apartment was from these touchstones. First, that required assigning coordinates to the addresses using Google's Geocoding API. From there, calculating distance was a breeze.
  
Now, we can look at these apartments on a heatmap

![apt_heatmap](https://github.com/mathyjokes/ApartmentList.com/blob/master/apartment_heatmap.png)


#### 3) Cleaning/Structuring
This step involved a few, ahem, *interesting* loops to get the data in the format I want. There were many simple steps as well, like breaking out the number of bedrooms, changing the data type of several columns to numeric, etc. The result can be seen below.

![apts_data_view](https://github.com/mathyjokes/ApartmentList.com/blob/master/apts_data_view.PNG)


#### 4) Initial Visualization/Exploration
This is always one of my favorite steps. I make several graphs and charts, without looking for anything specific to begin with. Although I understand the data from having cleaned it, this step let's me understand the limitations that the data may hold much better. It was in this step, for example, that I removed a clear outlier. An 8 bedroom apartment was available in the Georgetown area, which is one of the most expensive in the city. Since I definitely will *not* rent this apartment or anything like it (except in my daydreams), I can safely take it out of the data to create my model.

![sqft_price](https://github.com/mathyjokes/ApartmentList.com/blob/master/sqft_vs_rentalprice_no_outliers.png)


#### 5) Part of Speech Tagging for Building Amenities
Of course, even all the features in the data do not capture everything. Specific buildings have different amenities that are important to keep in mind when valuing an apartment. These amenities are advertised in descriptions buildings post about their apartments and location. Below, we can see the most common overall words, nouns, adjectives, and verbs used in the building descriptions. As many renters can tell you, buildings with renovated buildings with hardwood floors are in high demand. This trend can be seen in almost every part of speech category.

![word_frequency](https://github.com/mathyjokes/ApartmentList.com/blob/master/word_frequency_images/word_frequency.png)   |  ![noun_frequency](https://github.com/mathyjokes/ApartmentList.com/blob/master/word_frequency_images/noun_frequency.png)
:-------------------------:|:-------------------------:
![adj_frequency](https://github.com/mathyjokes/ApartmentList.com/blob/master/word_frequency_images/adj_frequency.png)  |  ![verb_frequency](https://github.com/mathyjokes/ApartmentList.com/blob/master/word_frequency_images/verb_frequency.png)


### Let's get back to business - what does all this mean for my apartment rental price?

#### 6) Random Forest Model

The question was not about apartment location or building desirability, however, but about apartment prices. So I go deeper into the data!

Random Forests are a collection of Decisions Trees. A supervised model, they are highly human-readable because they make "decisions" based on one feature at a time. They are useful for this analysis because they are robust against highly-correlated features and are not prone to overfitting.

In practice, a Random Forest model will take train on existing data to become attuned to the requirements of the analysis. In this case, I gave it the apartment rental information from ApartmentList.com to learn what an apartment will rent for given a set of features. At the end, a fully trained model can take in a list of features (like, for example, information about my own apartment) and return an estimated rental price.

So, I did just that. I did conduct some hyperparameter tuning to increase the accuracy of the model, but otherwise was ready to rock and roll fairly quickly. A look at which features are most important for price predication is below.

![feature_importance](https://github.com/mathyjokes/ApartmentList.com/blob/master/feature_importance.png)

Looks like apartment footage has the most important effect on apartment rental price prediction in DC. 
Proximity to the White House and to the Capitol building, variables added to indicate a measure of neighborhood desirability, have some of the smallest effects. 

## Finally, the results!

This whole excercise had a clear purpose: in the end, I want to know if I'm overpaying for my apartment.
So, I ran the model against my own apartment information.

The model predicts I should be paying ~$200 more than I do -- looks like I got a great deal! Guess I'll stick around a bit longer.
