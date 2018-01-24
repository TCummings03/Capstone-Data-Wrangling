## Springboard Data Wrangling Project

For this data wrangling exercise, I chose to use the data set for my capstone project. For my project, I chose to build a model with sci-kit learn that predicts rent prices in the DC area. Instead of looking for a pristine data set that was ready to be split into test/training sets and used to predict rent prices, I chose to go out and get the data on my own.  This meant that I had to do some webscrapping in order to get the data for rent prices in the DC area. I chose to use the beautifulsoup library to accomplish this.

After choosing a well known rent listing website that had listings for the DC area, I had to break up the webscrapping into a few parts:

1. The site had 20 listings per page and around 700 pages, so the first thing I did was get all 700 pages' url's and store them in a          list.
2. Next, I used each of the above pages to get preliminary information on each listing by scrapping the number of bedrooms/bathrooms,        latitude/longittude, listing id, sqft, and price. I stored each of these items in their respective master list. In addition, I            scrapped each listing's respective url to get further information on the listing.
3. After getting the preliminary information and each listing's url, I went a level deeper and scrapped in indivdual listing page.            From here, I got a list of all the amenities each listing had and appended it to a master list.
 4. Lastly, I created a dataframe with all the scrapped elements
    
Although the scrapping was finished, my work did not stop there because I needed to unpack the amenities list and turn them into dummy variables so they could be used in a machine learning model.  First, I got the unique amenities from the amenities list and turned each of those amenities into an empty column.  Second, I looped over each row in the amenities column and added a 1 to the respective column where the amenity appeared. After creating the dummy variables for amenities, I checked to make sure there were no duplicates, which would provide noise to the model.  I found that there were in fact duplicates and instances like 'Washer' and 'washer' were treated as two separate variables. To remedy this, I turned all the amenities columns into lower case, grouped by column and summed them.  In addition, I had to make sure all of the numeric data was of data type numeric, so that it could be used in sci-kit learn.  I used pd.to_numeric to achieve this.  

Geocoding: Since I was able to scrape the latitude and longitude data for each listing, I decided to geocode this data and retreive the locality for each listing.  This would, perhaps, provide more insight into the spread of the listings on a physical map, and potential predictive power in a machine learning model. For this, I used the pygeocoder package which is a wrapper for Google's geocoding API. 

Outliers:

During my exploratory data analysis, I created scatter plots of each of the numeric columns(beds, baths, sqft) plotted against rent price to see if there were any outliers. From the scatter plots, I was able to detect the outliers and investigate those points. I could tell that they were outliers on the graph becuase they did not follow the general trend. However, upon further inspection I was able to confirm they were outliers because they had values such as 99999 for square footage or price. I removed these points.  In addition, I checked points below 100 sqft that had rent prices that did not seem plausible -- I also removed these points. 

Missing Values:

The main issues concerning missing values had to do with the amenities column and the sqft column.  About 10% of listings did not include any amenities at all, whereas nearly 30% of listings did not include square footage.  For the amenities columns, I removed all listings without any amenities, so the model could present a better sense of which(if any) amenities had an effect on rent price.  For the square footage column, I decided to run a simpler linear regression model with bedrooms and bathrooms to impute the missing values.  This was my process for imputing missing sqft values:

1. Create a data frame with sqft, bedrooms, bathrooms
2. Get all observations where sqft = NaN
3. Drop all NaN values from the smaller data frame
4. Fit a linear regression model on the smaller data frame
5. Take the obersvations where sqft = NaN and predict NaN values with linear model
6. Add these predictions to the original data frame (fill in missing sqft values)

After I had removed outliers, checked for duplicates, imputied missing values, and created all dummy variables, my data frame was ready for a machine learning model. Through this process, I was able to get hands on experience with messy data from the web and turn it into a manageable dataframe that was accessible. 
    
