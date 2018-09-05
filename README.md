# Analyzing the data for a gaming company's most recent fantasy game Heroes of Pymoli.
Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. This project is to generate a report that breaks down the game's purchasing data into meaningful insights.

#### Tech/framework used:

* Pandas Library

* Jupyter Notebook

(the data has thousands of records. Hacking through the data to look for obvious trends in Excel is not a feasible option. The size of the data may seem daunting, but *Pandas* will efficiently parse through it.)

### Observable trends based on the data analysis:
* Of the 576 total players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%). 
* Although there is a huge difference in the games's gender demographic, there is not much difference between the amout each gender spends on average. On the contrary female players spend a little more than male players($4.5 compared to $4.0)
* Our peak age demographics falls between 20-24 (44.8 %) with secondary groups falling between 15-19 (18.6 %) and 25-29 (13.4 %). 

#### The final report includes the following:
* Player Count
  * Total Number of Players
* Purchasing Analysis (Total)
  * Number of Unique Items
  * Average Purchase Price
  * Total Number of Purchases
  * Total Revenue
* Gender Demographics
  * Percentage and Count of Male Players
  * Percentage and Count of Female Players
  * Percentage and Count of Other / Non-Disclosed
* Purchasing Analysis (Gender)
  * The below each broken by gender
    * Purchase Count
    * Average Purchase Price
    * Total Purchase Value
    * Average Purchase Total per Person by Gender
* Top Spenders
  * The top 5 spenders in the game by total purchase value:
    * SN
    * Purchase Count
    * Average Purchase Price
    * Total Purchase Value
* Most Popular Items
  * the 5 most popular items by purchase count:
    * Item ID
    * Item Name
    * Purchase Count
    * Item Price
    * Total Purchase Value
* Most Profitable Items
  * the 5 most profitable items by total purchase value:
    * Item ID
    * Item Name
    * Purchase Count
    * Item Price
    * Total Purchase Value

