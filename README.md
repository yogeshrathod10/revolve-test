import pandas as pd
from datetime import date


flights_df = pd.read_csv("C:/Users/HP/Desktop/assignments/data/flights.csv")
print(flights_df)
planes_df = pd.read_csv("C:/Users/HP/Desktop/assignments/data/planes.csv")
print(planes_df)
airports_df = pd.read_csv("C:/Users/HP/Desktop/assignments/data/airports.csv")
print(airports_df)
airline_df = pd.read_csv("C:/Users/HP/Desktop/assignments/data/airlines.csv")
print(airline_df)

# 1) how many departure cities (not airports) does the flights database cover?
dep_cities = flights_df.groupby("origin")["origin"].count()
print(dep_cities)
# There is only one departure city i.e. New York which has 3 major airports i.e.EWR,JFK,LGA.

# 2)how many total number of days does the flights table cover?


d0 = date(2013, 1, 1)
d1 = date(2013, 9, 30)
delta = d1 - d0
print(delta.days)
# 272 days

# 3)which airplane manufacturer incurred the most delays in the analysis period?
plane_manufacturer = planes_df.set_index("tailnum")["manufacturer"].to_dict()


def get_stats(group):
    return {
        "min_delays": group.min(),
        "max_delays": group.max(),
        "count": group.count(),
        "mean_delays": group.mean(),
    }


global_stats = (
    flights_df["dep_delay"]
    .groupby(planes_df["manufacturer"])
    .apply(get_stats)
    .unstack()
)
global_stats = global_stats.sort_values("count")
global_stats
# Ans-- Boeing manufacturer had the most delays i.e. 1623 times


# 4)What is the relationship between flights and planes tables?
# Ans)flights connects to planes via a single variable, tailnum.
# planes.tailnum is a primary key because it uniquely identifies each plane in the planes table.
# flights.tailnum is a foreign key because it appears in the flights table where it matches each flight to a unique plane


# 5)which are the two most connected cities?
dest_flight = flights_df.groupby("origin")["dest"].value_counts()

dest_flight.head(10)

# Ans) most connected cities are EWR and ORD
