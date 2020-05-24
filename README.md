# Data Description and Breakdown

There are two datasets being using in this project:

- [`covid19-newjersey`](https://github.com/saaqebs/covid19-newjersey): An _ongoing_ data collection of the number of COVID-19 cases per New Jersey municipalities from March 25, 2020 to Present.

- [`municipal.csv`](./data/municipal.csv): Basic population statistics and which county they are a part of for each NJ municipality.

At the moment, these are the two main datasets being used. I may incorporate median income for each township if I am able to find a good dataset describing this.

Let's dive into these datasets!

## COVID-19 New Jersey Data

#### Overview

The scripts for automating and scraping the designated articles for this data is located in [another repository](https://github.com/saaqebs/covid19-newjersey). More information about the collection, automation, and notes can be found there. 

The data is collected and recorded by different counties in NJ, which is then compiled and posted in articles by [NJ.com](https://www.nj.com/coronavirus/). 

#### Data Observations and Attributes

After scraping the data from the article, it is stored in a CSV file format for an ongoing compilation of the number of cases per municipality in New Jersey. The structure of the file is as such:

| Municipal | County        | Cases | Date          |
|-----------|---------------|-------|---------------|
| allendale | bergen county | 4     | march-25-2020 |
| alpine    | bergen county | 1     | march-25-2020 |
| ...       | ...           | ...   | ...           |

The "Date" column is structured as `{month name}-{day}-{year}`. The "Municipal" and "County" variable contains the official municipality name with its corresponding county as posted in [`nj_municipals.json`](./data/nj_municipals.json). The "Cases" column is simply an integer indicating the number of cases.

#### Notes

As of April 26, all counties have been reporting township case numbers. However, some counties began disclosing the data later than other counties (eg. Atlantic and Mercer County), creating a discord in the data. 

There was no publication of data for May 1, 2020 on NJ.com. 

I recently found a bug where on 3 dates (5/8, 5/15, 5/21) where they split the data publishing into multiple days, affecting the automated data parsing. 

<!-- ![#ffffff](https://via.placeholder.com/15/ffffff/000000?text=+)

![#ffffff](https://via.placeholder.com/15/ffffff/000000?text=+) -->

## New Jersey Municipality Data

#### Overview

This data contains some basic information for each municipality in New Jersey. 

The data is grabbed from this [Wikipedia Article](https://en.wikipedia.org/wiki/List_of_municipalities_in_New_Jersey)
and stored it in CSV file named `municpial.csv`. 

#### Data Observations and Attributes

After scraping the data from the Wikipedia entry, it is stored into a CSV file as such:

| Municipal  | County    | Population | Type     |
|------------|-----------|------------|----------|
| newark     | esssex    | 285154     | city     |
| edison     | middlesex | 102450     | township |
| somerville | somerset  | 12418      | borough  |
| ...        | ...       | ...        | ...      |

The attributes for this dataset is the "Municipal", "County", "Population", and "Type". The "Population" column is the estimated population of the municipality in 2017. The "Type" column contains whether this municipality is a "city", "township", "borough", etc.

#### Notes

I hope to expand this dataset and include a median income column for each municipality to further explore the analysis of COVID-19 cases in NJ.


## Data Scripts

There are three main Python Notebook files that have code that has been used to aggregate or analyze the data above. 

- [`Analytics.ipynb`](./Analytics.ipynb): Contains the preloaded data frames of the described datasets from above.

- [`NJMunicipalFullData.ipynb`](./scripts/NJMunicipalFullData.ipynb): Scrapes the Wikipedia entry for NJ Municipality data.

- [`NJCovid19Data.ipynb`](./scripts/NJCovid19Data.ipynb): Scrapes NJ.com article for COVID-19 cases for one day. I have a seperate script located in [this repository](https://github.com/saaqebs/covid19-newjersey) to automate the collection of data daily. 

More details can be found in those specific files.

## Significance

### Case Rates Brings Numbers Down to Earth

What was the most interesting to me was how the case rates in most municipalities (with the exception of 1 municipal at 25%) hovered at 1.5%. Since New Jersey only began testing asymptomatic cases last week, it makes sense that the positivity rate is extremely low. An observation regarding this is the fact how the standard deviation and variance for the case rate is extremely low (Variance is 0.02%!).

This shows while it is _extremely frightening_ to see a high number of cases Franklin Township  than in South Brunswick (452) (neighboring townships), the case rates for each  townships is still hovering around 1-2% (1.7% and 0.97% respectively).

![](documents/collective.png)
*Comparing Central Jersey case numbers. More specifically the difference between two neighboring towns: South Brunswick and Franklin Township.*

Further proving this is a histogram illustrating the distribution of case rates. The histogram excludes two outliers: Woodland Township (13.5%) and Rockleigh (27.1%).

![](documents/histogram.png)
*A histogram displaying the distribution of case rates.*

This histogram ultimately shows that most municipalities hover around 1% case rate.

One thing to note however is the fact that 1% of 45,000 (South Brunswick) is a much bigger number of people than 1% 67,000 people (Franklin Township). The difference is $670 - 450 = 220$ more people.

A prediction: With a massive increase of testing, the number of asymptomatic case being tested as positive will rise will cause the average case rate to rise up to 4% of the municipal's population.

### Case Rate and Municipality Type Relationship

Since COVID-19 is a respiratory disease, it only makes sense for denser populated areas to have a higher outbreak of the virus. This prompted me to analyze the relationship between case rate and the different types of municipalities. 

The analysis showed that Boroughs and Townships had low case rates regardless of populations. This may be a result of the nature of boroughs and townships; these types of municipalities have their populous spread across a large area, limiting the close interactions between groups of people.

![](documents/stable.png)
*Observing the relationship between Townships/Boroughs.*

The most promising is with City; the Spearmans's correlation is 0.80, showing that there some sort of correlation. In the figure below, it is quite clear that the two variables are positively correlated 