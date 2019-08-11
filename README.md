# ETL Project Report (# of gun owners and votes in last 2016 election) 

/Users/ufukeminkoroglu/Desktop/Image.jpeg

## Project Proposal

Our aim, in this project, to compare the number of gun owners and votes that each party received in the 2016 presidential election per state. The aim is to compare blue and red states in terms of gun ownership per capita. 

## Finding Data

In this project, we used two sources of data. We used the following sites to use as sources of our data:

* https://www.usa.gov/election-results (CSV file)

* https://www.thoughtco.com/ (converted to CSV)

* https://gist.github.com/mshafrir/2646763


## Data Cleanup & Analysis

Once we have identified our datasets, we performed ETL on the data. We planned and documented the following:

### Extract CSVs into DataFrames

* We extracted CSVs (2016 election results per states and # of gun owners) into DataFrame.

* We realized that in one of our data set, state names were not listed as abbreviations. We found CSV format of states that included states abbreviations. 

* We merged that with 2016 elections per states, and then we dropped the column of the state that showed full names. As a result of that, both CSV files had the state name by their abbreviations.

### Transform elections DataFrame
#### election data
* Create a filtered dataframe from specific columns ("State ", "Trump (R)", "Clinton (D)", "All Others")

* Renamed the column headers as (State ':"State", 'Trump (R)':"Trump", 'Clinton (D)':"Clinton"})

* Set index as election_transformed.set_index("State", inplace=True)

#### gun owner's data
* Create a filtered dataframe from specific columns
gun_cols = ["# of guns per capita" , "# of guns registered","Abbreviation"]

* Rename the column headers
gun_transformed = gun_transformed.rename(columns={'# of guns per capita':"num_guns_per_cap", '# of guns registered':"num_guns_regist", 'Abbreviation':"State"})

* Set index as gun_transformed.set_index("State", inplace=True)

## Create a database connection

* We created a database (titled as gun_election_relation) through PgAdmin4

connection_string = "postgres:abcd1985!@localhost:5432/gun_election_relation"

engine = create_engine(f'postgresql://{connection_string}')

### Confirm tables
engine.table_names()

## Load DataFrames into the database

* We then loaded the data by the following queries

election_transformed.to_sql(name='elections_2016', con=engine, if_exists='append', index=True)

gun_transformed.to_sql(name='gun_numbers', con=engine, if_exists='append', index=True)

