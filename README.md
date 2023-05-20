# Job-Description-Data
  The goal of this project is to explore and analyze the job descriptions of data engineering job listings.
  
**Tools/Skills Utilized**
-ETL (Extract, Transform, Load)
-extract via REST API
-Clean, wrangling, transform data using python and pandas and other libraries
-Load onto Postgres DB
-Build Visualizations of the data with tableau and Power Bi
-Collaborate using GitHub



# Details

1. [Data](#data)
2. Extraction
3. Cleaning
4. Transforming
5. Analyzing


# Data

  We wanted to explore the verbal structure and components found in job descriptions/requirements on job listings specifically for the data engineering field.  We explored a few apis from different job listing sites and settled on TheMuse.com'a api because it was one of the only ones that provided the full job description directly in the api instead of a truncated snippet.  
  Although the api was the best for our needs it had a few downsides:
    -The api didn't return as many listings as other sources
    -When searching across a large area, hundreds of pages were returned but the api wouldn't allow us to access pages nubmered 100 or more
    -When searching across a specific area, remote jobs from other parts of the world would be returned.  This would distort the data since we were primarily focused on job just in USA.


