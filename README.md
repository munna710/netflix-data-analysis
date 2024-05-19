# Netflix Data Analysis Project

This project involves analyzing a dataset of Netflix titles to gain insights into various aspects such as genre distribution, director contributions, and country-specific trends. The data is first cleaned and transformed, then stored in a SQL Server database for further analysis.

## Table of Contents

- [Dataset](#dataset)
- [Setup](#setup)
- [Data Cleaning](#data-cleaning)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Queries](#queries)


## Dataset

The dataset used in this project is `netflix_titles.csv`, which contains information about Netflix titles including movies and TV shows. The dataset can be downloaded from [Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows).

The dataset has the following columns:

- `show_id`
- `type`
- `title`
- `director`
- `cast`
- `country`
- `date_added`
- `release_year`
- `rating`
- `duration`
- `listed_in`
- `description`

## Setup

### Requirements

- Python
- pandas
- SQLAlchemy
- SQL Server with ODBC Driver 17 for SQL Server

### Steps

1. Clone this repository:
    ```sh
    https://github.com/munna710/netflix-data-analysis.git
    ```

2. Install the required Python packages:
    ```sh
    pip install pandas sqlalchemy pyodbc
    ```

3. Ensure you have SQL Server installed and running. Adjust the connection string in the script if necessary.

## Data Cleaning

The script `data_extact.ipynb` performs the following tasks:

1. Reads the dataset into a pandas DataFrame.
2. Establishes a connection to the SQL Server database.
3. Writes the DataFrame to a table named `netflix_raw` in the database.
4. Removes duplicate records based on `show_id` and `(title, type)` combination.
5. Creates separate tables for genres, directors, countries, and casts by splitting the respective columns.

## Data Analysis

The analysis is performed using a series of SQL queries to extract meaningful insights from the cleaned data. The queries are included in `SQLQuery1.sql`.

### Key Analyses

1. **Count of Movies and TV Shows per Director**:
    - For directors who have created both movies and TV shows.

2. **Country with the Most Comedy Movies**:
    - Identifies the country with the highest number of comedy movies.

3. **Top Directors by Year**:
    - For each year, identifies the director with the most movie releases.

4. **Average Movie Duration by Genre**:
    - Calculates the average duration of movies for each genre.

5. **Directors Creating Both Comedy and Horror Movies**:
    - Lists directors who have created both comedy and horror movies along with the count of each.

## Results

### Sample Outputs

- **Count of Movies and TV Shows per Director**:
    ```sql
    SELECT nd.director, 
           COUNT(DISTINCT CASE WHEN n.type = 'Movie' THEN n.show_id END) AS no_of_movies,
           COUNT(DISTINCT CASE WHEN n.type = 'TV Show' THEN n.show_id END) AS no_of_tvshow
    FROM netflix n
    INNER JOIN netflix_director nd ON n.show_id = nd.show_id
    GROUP BY nd.director
    HAVING COUNT(DISTINCT n.type) > 1;
    ```

- **Country with the Most Comedy Movies**:
    ```sql
    SELECT TOP 1 nc.country, COUNT(DISTINCT ng.show_id) AS no_of_movies
    FROM netflix_genre ng
    INNER JOIN netflix_country nc ON ng.show_id = nc.show_id
    INNER JOIN netflix n ON ng.show_id = n.show_id
    WHERE ng.genre = 'Comedies' AND n.type = 'Movie'
    GROUP BY nc.country
    ORDER BY no_of_movies DESC;
    ```

## Queries

The SQL queries used for data analysis are provided in the `SQLQuery1.sql` file. They include various tasks such as removing duplicates, creating normalized tables, and performing analytical queries.


