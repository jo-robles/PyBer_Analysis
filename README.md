# PyBer_Analysis

## Overview of the analysis:

Utilizing data from PyBer, we have been tasked to review fare data presented within a CSV file. From this analysis, we utilize Pandas and Matplotlib to create the following:

1) A DataFrame containing:
    1) The total values of:
        1) Rides per city type
        2) Drivers per city type
        3) Fares per city type
    2) Averages of:
        1) Fare per ride per city type
        2) Fare per driver per city type

2) A graph that summarizes the weekly fares per city type

Based on the information contained, we reviewed the data for trends and differences between the city types and how PyBer can capitalize on this information. 

## Resources Utilized

**Data Sources (Resource Folder):**

city_data.csv - CSV file containing the following data: city name, city type (Urban, Suburban or Rural) and the number of drivers. 

ride_data.csv - CSV file containing the following data: city, date/time the ride occurred, the fair and the ride ID. 

**Software Utilized:** Python 3.7.6, Anaconda 4.10.1, Jupyter Notebook, Pandas, Matplotlib

## Results:

To begin our analysis, we first began by translating our CSV files into a Pandas DataFrame and then merging them utilizing the following code:

```
city_data_to_load = "Resources/city_data.csv"
ride_data_to_load = "Resources/ride_data.csv"

#Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)

#Creating the merged DataFrame
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])
```
From here, in order to create our final DataFrame, we utilized the ```.groupby``` function to pull out the data we required from our merged DataFrame. An example of the code we utilized is as follows:

```
#Get the total amount of fares for each city type
total_fares_city_type = pyber_data_df.groupby(['type']).sum()['fare']
```

After we gathered our summary data, we merged all of these into one DataFrame:

```
pyber_summary_df = pd.DataFrame({
    'Total Rides':total_rides_city_type,
    'Total Drivers':total_drivers_city_type,
    'Total Fares':total_fares_city_type,
    'Average Fare per Ride':average_fares_ride_type,
    'Average Fare per Driver':average_fares_city_type
})

```
And, after formatting, we show the following:

![](https://github.com/jo-robles/PyBer_Analysis/blob/ea391ace40e3e607c139bd2dd6405dad62e86c72/Analysis/PyBer_summary.PNG)

Based on this merged DataFrame, we can draw the following conclusions about our PyBer data:

1) Although there are significantly more Urban Rides and Drivers, the average fare per ride and per Driver is lower than the other two categories. We can make an assumption then that Urban drivers, while more frequent, are simply not taking longer routes or more time in their driving as compared to other city types. However, the sheer volume of drivers/riders (and total fares) makes this the most lucrative portion of the business.

2) Rural fares per driver are the highest among the city types. Considering that most individuals who would be utilizing a ride service would be travelling longer distances (as a result of being rural), this stands to reason. 

3. The suburbs seem to occupy the "middle" ground between the two extremes that are present. Perhaps these individuals are utilizing the ride-service to take rides that are considered more "occasional" or on special occasions. 

In addition to our summary DataFrame, we also sought to understand how weekly fares stacked against each of the city types presented within our data. To do so, after some initial drilling down, we created a PivotTable from a DataFrame focused between January - April utilizing the ```.loc``` function:

```
Jan_April_2019_df = date_fares_pivot.loc['2019-01-01':'2019-04-29']
```

And then set our index to datetype and then resampled by Weeks:

```
#Setting the date indext to datetime datatype
Jan_April_2019_df.index = pd.to_datetime(Jan_April_2019_df.index)

#Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
fares_by_weeks = Jan_April_2019_df.resample('W').sum()
```

Now that we had our DataFrame, we could create the plot utilizing an object-oriented interface method:

```
# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')

fare_by_city_type_plot = fares_by_weeks.plot(figsize=(15,5))
fare_by_city_type_plot.legend(loc='center')
fare_by_city_type_plot.set_xlabel('')
fare_by_city_type_plot.set_ylabel('Fare ($USD)')
fare_by_city_type_plot.set_title('Total Fare by City Type')
plt.savefig('Analysis/PyBer_Fare_summary.png')
```

In our code above, we utilized the FiveThirtyEight style, set our labels and title and finally, saved our graph to the Analysis folder by the name 'PyBer_Fare_Summary.png'

![](https://github.com/jo-robles/PyBer_Analysis/blob/ea391ace40e3e607c139bd2dd6405dad62e86c72/Analysis/PyBer_fare_summary.png)

From this graph, we can draw the following conclusions:

1) Although the average fare per driver and rider was significantly higher for the Rural city type, when we consider the fare type by city on a weekly basis, we see that the Rural city type is significantly lower each month. In consideration from the previous DataFrame summary then, although the average fare of the Rural city type is much higher than other types, the actual revenue that is brought in per month is lower. 

2) On the exact opposite side of the spectrum, we have the Urban fare types. While the average fare per driver/rider is lower than other city types, we see that the total fares brought in are higher per week than other city types. This means, that the sheer volume of riders/drivers we see for the urban city types make up for any lower average amounts.

3) And similarly, as before with the averages, we see that the Suburban city types maintain mostly within the middle with occasional spikes as we move past April and slightly before March.

## Summary:

Based on the information presented above, the following stand as three business recommendations to address disparities among the city types:

1) In consideration of the total fares brought in per week, PyBer would do well to consider maintaining a strong presence within urban city types. A further analysis of the reasons why the average fare per driver is lower than other city types would be warranted. Although it is suspected that the length of the ride is shorter as compared to other city types, a deeper analysis would either substantiate or disregard this.  
2) For the Rural city type, and in consideration of the average fare per driver, there seems to be an opportunity to do more marketing or increase the presence of PyBer in this area. However, what remains to be seen is the exact utilization of these services if presence is increased. We see this concern manifest in the total fares per week as compared to other city types. Therefore, while more brand awareness and marketing could certainly help, will this translate into more usage? Further analysis would be warranted. 
3) For the suburban city types, a small spike in the total fares is observed between the months of February and March and dip is observed in the later part of April. These two instances stand out as otherwise, the suburban city type tends to be stable. Further analysis would be warranted to determine the reasoning behind the small spike and small dip in their respective months. Understanding these causes could provide some direction of how PyBer should either react or what they should fix.
