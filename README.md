# PyBer_Analysis

The purpose of this analysis is to create a better understanding of the differences between city types for PyBer Ride Sharing. Using data from a set timeframe, we review the number of drivers, the number of rides, the income from fares and compare them between one another. This analysis allows us to provide suggestions for changes to improve future business practices to increase company growth.

## Project Overview

To begin, I imported the provided data and merged them to create a new dataframe. Then, using the groupby() function on the city type, I pulled the total number of rides and drivers, and the sum of fares. This allowed me to calculate the average fare by ride and by driver for Rural, Suburban, and Urban city types. See the code below.

```
#  1. Get the total rides for each city type
total_rides = pyber_data_df.groupby("type").count()["ride_id"]
total_rides

# 2. Get the total drivers for each city type
total_drivers = city_data_df.groupby(["type"]).sum()["driver_count"]
total_drivers

#  3. Get the total amount of fares for each city type
total_fares = pyber_data_df.groupby("type").sum()["fare"]
total_fares

#  4. Get the average fare per ride for each city type. 
average_fare_ride = total_fares / total_rides
average_fare_ride

# 5. Get the average fare per driver for each city type. 
average_fare_driver = total_fares / total_drivers
average_fare_driver
```

Extracting these figures allowed me to create an initial comparison between Rural, Suburban, and Urban cities as can be seen in the summary dataframe below:

![Summary Dataframe](https://github.com/ChallahBack83/PyBer_Analysis/blob/main/Analysis_Challenge/PyBer_summary_df.png)

However, I was also tasked with creating a comparison of fare by week for each city type. Taking the initial merged dataframe, I grouped by both city type and date before pivoting the data so that the columns became the city type and the fares were then indexed by date. This pivot allowed me to isolate a section of the data by date and resample that section into weeks. As the following code and image show:

```
# 4. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-28'.
pyber_grouped_fares_date = pyber_grouped_fares_pivot.loc["2019-01-01": "2019-04-28"]
pyber_grouped_fares_date

# 5. Set the "date" index to datetime datatype. This is necessary to use the resample() method in Step 8.
pyber_grouped_fares_date.index = pd.to_datetime(pyber_grouped_fares_date.index)
pyber_grouped_fares_date

# 7. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
weekly_pyber_grouped_fares = pyber_grouped_fares_date.resample("W").sum()
weekly_pyber_grouped_fares
```

![Fares By Week Dataframe]()

With the weekly fares now gathered, I could create a multi-line graph that showed the weekly fares overtime for Rural, Suburban, and Urban city types.

## Results

Looking at the Summary Dataframe, we can see the stark difference between the three city types. Urban cities have the highest number of rides and drivers which leads to a higher Total Fares. Rural cities are dramatically lower in numbers with only 125 Rides and 78 drivers in comparison to the Urban cities 1,625 rides and 2,405 drivers.  However, the average fare per ride and driver is highest for Rural rides and drivers. Considering the probable difference in drive time and size of populations, these figures are not entirely unsurprising.  Rural communities obviously have destinations further away and less people to pull from.  Therefore, on average their fare amounts will be higher even though their total income will remain lower than both their Urban and Sububan counterparts.

![Summary Dataframe](https://github.com/ChallahBack83/PyBer_Analysis/blob/main/Analysis_Challenge/PyBer_summary_df.png)

The following graph summarizes the total fare by week for each city type looking at a four month time frame. This allows us to take a more focused look at the differences between Urban, Suburban, and Rural communities.  It supports the assumption from our summary dataframe that Urban cities generate more income than Suburban and Rural cities. It also shows spikes in total fares and at least one that hits across all city types, suggesting there is something influencing an increase during that week (2019-02-24), which we may want to investigate further.

![PyBer Fare Summary](https://github.com/ChallahBack83/PyBer_Analysis/blob/main/Analysis_Challenge/PyBer_fare_summary.png)

## Summary with Recommendations

In summary, PyBer is currently making more income from Urban rides than either Suburban and Rural rides, but this is mostly due to the volume of rides and not due to pricing. The graph also shows trends in higher fares at certain times. With further investigation, we may gain better insights to these trends and beable to utilize them to further increase profits.  

My initial recommendations are:

1. Compare the number of rides by week to the total fares by week to understand if the difference is because of the volume of rides or distance of the trip.

2. Take a look at the events or holidays during a set timeframe and compare them to the trends in number of rides or total fares. PyBar could capitalize on these high volume timeframes with short term increases in fees or promotional materials to generate more revenue.

3. Increase the fares for cities above a specific population or with a certain threshold of average rides. PyBar can increase revenue in regions with high traffic.
