# DATA601-HW1
DATA 601 Homework 1

# Overview and Goals
For this project, I chose to perform data analysis on Baltimore City crime data.  The motivation in exploring this data is to find patterns and trends in crimes so that we may learn more information and be better prepared to stop crime.  We could even run some predictive analytics on the data to predict where/when/how a crime might happen next, but for the exploratory purposes of this homework will hold off on those for now.  As a result, police officers may be stationed at those specific places at the time crime is expected, and be looking for signs of the type of crime expected to happen.  If they are aware that it will happen before it does, the goal is for them to be able to prevent it before it happens.

# How to Navigate
[Data](https://github.com/zvance1/DATA601-HW1#data) -
[Methods](https://github.com/zvance1/DATA601-HW1#methods) -
[Conclusions](https://github.com/zvance1/DATA601-HW1#conclusions) -
[Project Info](https://github.com/zvance1/DATA601-HW1#project-info) -

The data in its CSV format is located in the data directory of this repository.  Also in this data directory is the csv I wrote after cleaning the data.  In the notebooks repository you will find three jupyter notebooks - one for the loading and cleaning of the data, one for the exploring of the data, and lastly the technical report.  The images directory contains the visualizations used for analyzing the data.

# Data
To perform this analysis, I will use a data set named "BPD Part 1 Victim Based Crime Data" from Open Baltimore (Here: https://data.baltimorecity.gov/Public-Safety/BPD-Part-1-Victim-Based-Crime-Data/wsfq-mvij/data).  This data is updated every Monday, with a 9 day time lag to minimize changes to the data as records move throughout the BPD review process.  I exported this data as a CSV file and began the cleaning of it from there.  The data contains 16 columns and 313,634 rows.  This should be plenty of data to get a good idea of trends and potentially for predictive analytics as well.  One limitation for predictive analytics may be that this data is very qualitative, with most values being strings input into machine learning algorithms may be slightly more difficult depending on the algorithm.  A concern is also being able to perform exploratory analysis on things like the address and premise, as these may prove important but difficult to analyze at a large general scale.

# Methods
Questions to be answered:
   * Is there a trend between time of day and number of crimes?
   * Is there a trend between the time of day and weapon used?
   * Is there a trend between the month of the year/season and the number of crimes?
   * Is there a trend between the day of the week and the number of crimes?
   * Is there a pattern in the number of crimes inside vs. outside based on the time of year?
   * Is there a pattern in the location of a crime (inside vs. outside) based on the weapon used?
   * Is a certain weapon more likely, given whether the crime was inside or outside?
   * Is there a pattern in the weapon used based on district?
   * Is there a pattern in the weapon used per description?

Techniques:
I use pandas to import, clean, and explore the data after importing from excel into a dataframe.  The first step after reading the CSV file that was exported straight from Open Baltimore is to clean the data in preparation for analysis.  Visualizations are created for each of the above questions (in the technical report notebook) to help explain the trends in the data.

Cleaning the data:
The following are the steps, techniques and assumptions I used to clean the data:
1.  I use pandas read_csv to read the csv file which had a 'utf-8' encoding.
2.  I look at the column names to analyze which columns are good for answering our questions, and which contain duplicate information/ are not valuable and can be gotten rid of.  The techniques I use here are the pandas dataframe info() and head() functions to gain a better understanding of the data.  I also scrolled through the csv in excel to eyball the data quickly to get a better understanding of it.
3. I use the rename function with a lamda function to replace the spaces and slashes with an empty string to get rid of them.  
4. After looking at head(), I see that it looks like the TotalIncidents column is always 1, so I count the number of values in that column equal to one and turns out, it is the same number as there are rows (313,634), meaning they are all 1, which tells us nothing and this column may be dropped.  
5. It looks like the contents of the vri_name1 column are contained in the District column.  That, combined with the fact that there are only 37,056 non-null values in the vri_name1 column leads me to the decision that it can be dropped for simplicity.  This should be noted that our results will not have any analysis of the vri_name1 column (some are slightly different than the district and this content will not be represented, we deem it unimportant for the time being but it could come into play later. 
6. After looking at the output of the info() function, it is determined that the column, Location1, has 0 non-null values and can obviously be dropped.
7. I drop those 3 columns (Location1, vri_name1, TotalIncidents) using the pandas drop function.
8. Now that the columns are set, I look at handling the missing values and inconsistencies. The data has 313,634 entries.  I look at the output of the pandas info() function to gain more information from about the numbers of null values.  The columns of CrimeTime, Location, InsideOutside, Weapon, Post, District, Neighborhood, Longitude, Latitude, Premise all contain null values.  
    * The weapon column only had 66,908 non-null values, with so many nulls, I made the assumption that a null weapon means no weapon was used and filled them with 'None' using the pandas fillna function.
    * CrimeTime and Post each had small amounts of null values, and there is not a great way to fill them without introducing bias to the data.  For example, filling the missing times with midnight would increase the number of crimes at midnight to more than they should be when doing time analysis.  So, any row with a null for CrimeTime or Post, I drop.
    * The remaining columns of Location, InsideOutside, Neighborhood, and Premise are all strings and we can easily replace their nulls with 'Unknown' so that we can keep those data rows, and at the same time keep in mind that there are unknowns when doing the analysis.
9. I clean other inconsistencies such as FIRE to FIREARM and I to Inside, O to Outside.
10. I write the cleaned pandas dataframe to a csv file to be loaded from the explore workbook.  For the code of this cleaning, please see the load_and_clean workbook.

# Conclusions
Each question was answered through analysis of the data. In summary:
* There is a trend between the time of day and the number of crimes, with the minimum amounts of crime happening between 4am and 6am, rising relatively steadily throughout the day and then hits a maximum around 4pm with the number remaining higher through roughly 10pm, and another little spike around mindnight (12am/ hour 0). In general crimes tend to peak when the highest number of people are out and about. Visualization here:
![crimes_per_hour.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/crimes_per_hour.png)
* There is a trend between time of day and weapon used - the biggest takeaway is that firearms are used more in the later hours of the night. Visualization here:
![weapon_per_time.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/weapon_per_time.png)
* There is a trend between month of the year/season and the number of crimes - warmer months tend to have more crime. As more people tend to stay put and indoors in the colder months, less crime takes place.  Visualization here:
![crimes_per_month.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/crimes_per_month.png)
* There is not a deliberate trend between day of the week and crime - if anything, slightly more crime happens during the week and we say that this may be due to the fact that Baltimore may be higher populated during the week as all the business professionals work weekdays.  Visualization here:
![crimes_per_day.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/crimes_per_day.png)
* There is a pattern in the number of crimes inside vs. outside based on the time of year - in the warmer months more crimes take place outside; in the colder months, more crime tends to take place indoors.  Visualization here:
![inside_outside_per_month.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/inside_outside_per_month.png)
* There is a pattern in the location of the crime based on the weapon used. When a firearm is used, it is more often than not used outside. On the other hand, when hands are used it is more often than not used inside. So, police should be aware of this fact and understand when a call for a shooting comes in, it is likely an outdoor act.  Visualization here:
![weapon_per_location.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/weapon_per_location.png)
* There is a pattern in the location (inside vs. outside) and the weapon used - given that a crime is outside, over 60% of the time, a firearm was used, whereas a knife ~15% of the time, and hands ~5% of the time. On the other hand, when a crime is commited inside, a firearm was used roughly 33% of the time, knife ~20% and hands ~10%. Police should have these numbers in mind when stationed to spot crime to know what is likely to happen given the location/scenario they are in. Visualization here:
![location_per_weapon.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/location_per_weapon.png)
* There is not a distinct pattern in the weapon used based on district - each district has a relatively similar distribution of weapons used, and police of all types should exist in all districts.  Visualization here:
![weapon_per_district.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/weapon_per_district.png)
* For the description of each crime, we can see the distribution of each weapon used.  The conclusion is that police will be able to know what weapon, if any, is likely to have been used or to be used given a particular crime took or might take place in the future. Visualization here:
![weapon_per_description.png](https://github.com/zvance1/DATA601-HW1/blob/master/images/weapon_per_description.png)

The possibilities for exploration of this dataset are endless, these are some of the trends I saw as most important for police officers. Continued exploration could involve setting up a chat with an officer to determine what trends and analysis would actually be most helpful to them.

# Project Info
<pre>
Contributors : <a href=https://github.com/zvance1>Zach Vance</a>
</pre>

<pre>
Languages    : Python3
Tools/IDE    : Anaconda, Jupyter Notebook
Libraries    : pandas
</pre>

<pre>
Duration     : September 2020
Last Update  : 09.17.2020
</pre>
