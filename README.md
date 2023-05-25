# Job-Description-Data
  The goal of this project is to explore and analyze the job descriptions of data engineering job listings.
  
  
  
**Tools/Skills Utilized**
* ETL (Extract, Transform, Load)
* Extract via REST API
* Clean, wrangling, transform data using python and pandas and other libraries
* Load onto Postgres DB
* Build Visualizations of the data with tableau and Power Bi
* Collaborate using GitHub



# Details

1. [Data](#data)
2. [Extraction](#extraction)
3. Cleaning/Transforming(#cleaning/transforming)
4. Analyzing


# Data

  We wanted to explore the verbal structure of job descriptions/requirements on job listings, specifically for the data engineering field.  We explored a few apis from different job listing sites and settled on TheMuse.com'a api because it was one of the only ones that provided the full job description directly in the api instead of a truncated snippet.  
  
  Although the api was the best for our needs it had a few downsides:
  
   1. There weren't as many listings as other sources.  
   2. The complementary data was pretty shallow.  Other sources returned a few more data fields.  For example, other api sources had specific salary ranges but the only similar information supplied with this api was entry level, mid level, or senior level.


# Extraction

  There were a few issues that hindred the extraction process from going smoothly:
   1. When searching across a large area, hundreds of pages were returned but the api wouldn't allow us to access pages nubmered 100 or more.  For example, if we searched with no location parameter we would get 667 pages of results across the world but we could only access the first 100 pages.
   2. The api didn't allow to search based on state or country, only specific cities.  And there were thousands and thousands of cities to pull data from.  We could specify a single city in the location parameter and repeat the process for each possible city but there were thousands of American cities so this would be a cumbersome process.
   3. When extracting results from a specific city, remote jobs from other parts of the world would be returned.  This would distort the data since we were primarily focused on job just in USA.  Additionally it returned a lot of redundant data.  For example, searching in just "Chicago, IL" returned 52 pages of results.  Meanwhile, performing the same search in "Aberdeen, SD" yielded 44 pages of results. Most of these results overlapped due to remote jobs appearing no matter the location specified.

  To address these issues we came up with a few clever solutions.  First we scraped the api documentation using the BeautifulSoup module to get a list of all the cities that are able to be passed as a parameter. Then, using regex, we eliminated the international cities (the american cities all had a state abbreviation after the comma), which resulted in a list of nearly 2400 cities.  
  
  We learned that we could pass in multiple locations into the locations parameter which reduced the amount of redundant results due to remote jobs appearing in every locations' results.  So we passed in 600 locations at a time which resulted in about 110-150 pages, depending on which 600 cities were passed in the parameters.  Since we couldn't access past page 100 we decided to look at the resulting pages in both ascending and descending order.  This allowed us to iterate through the pages from both ends and meet in the middle, encompassing all the results.  
  
  We saved each job listing to a Python dictionary using the listing id as a key.  Since dictionaries cant have duplicate keys this process removed any duplicate listings that may have appeared in the results.  We saved the dictionary to a json file so that we can easily load it for the transformation process. 

# Cleaning/Transforming

  First we dropped the uneccessary columns: type, model_type, short_name, and refs.
  We changed each row's data in the locations column to be a list of the locations.  The levels column's data was changed to be just the "short_name" value (i.e. interniship, entry, mid, senior).  The company column's data was shortened to just the company's name.  The publican_date column was split into two columns, one with the date and one with the time.
  We used the BeautifulSoup module to remove the html tags from the contents column's data and then we used regex to remove and newline (\n) or tab (\t) characters.  
