# Pandas Cheat Sheet

## Common packages
```import pandas as pd```

## Create a DataFrame by importing a CSV

**Pass the relative or absolute path**

File in current directory

    first_df = pd.read_csv("cartoonDogs.csv")

For this to work, the directory called "docs" must exist in the current directory

    my_new_df = pd.read_csv("docs/stuff/things/file.csv")

Or give an absolute path from the file system root

    awesome_df = pd.read_csv("/c/Users/jschmo/confidentialData.csv")

You may also have to include encoding when reading a csv

    just_ok_df = pd.read_csv("my_aight.csv", encoding="utf-8")

You may want to suppress a low memory warning using the low_memory parameter

    quite_large_df = pd.read_csv("/c/Users/jschmo/lotsaData.csv", low_memory=False)

## Save a DataFrame to a file

### Write a CSV

* index parameter is a boolean that indicates whether to include the index in the output file

* header parameter is a boolean that indicates whether to write the column names to the file

    file_one_df.to_csv("Output/fileOne.csv", index=False, header=True)

### Write an Excel file
    file_one_df.to_excel("Output/exc")

## Get the type of a variable

**Use the python type() function**

    type(my_df)
    type(my_df["column named earl"])
    type(some_variable)


## Create a Series

**Pass a list by variable**

    universities = ["UCLA", "UC Berkeley", "UC Irvine"]
    data_series = pd.Series(universities)

**Pass a list directly**

    secretariesOfState = pd.Series(["Albright", "Powell","Rice","Clinton"])


## Create a DataFrame

**Pass a list of dictionaries.** Each dictionary is a "row"

    states_df = pd.DataFrame([{"STATE": "New Jersey", "ABBREVIATION": "NJ"},
                            {"STATE": "Alabama", "ABBREVIATION": "AL"},
                            {"STATE":"Illinois", "ABBREVIATION": "IL"}])

**Pass a dictionary of lists.** Each list (dictionary value) is a Series

    pharaoh_df = pd.DataFrame(
        {
            "Dynasty": ["Early Dynastic Period", "Old Kingdom", "Tut Times"],
             "Pharaoh": ["Thinis", "Memphis","Tut"]
        }
    )

**Pass a DataFrame** This does *not* make a copy of a DataFrame. It just gives another handle to the original DataFrame.

    dutch_df = pd.DataFrame(holland_df)

**Specify which columns to include** Use the *columns* parameter to give a list of columns. *Why?* For example, if your objects have 50 fields, but you only need to use 4 of them.

    homesList = [
        {"Living Area":100,"Sewer":"Overhead","Bedrooms":4,"Last Sale Date":"4/4/2004", "Last Sale Price":100000,"Tax":5000},
        {"Living Area":100,"Sewer":"Overhead","Bedrooms":3,"Last Sale Date":"4/4/2005", "Last Sale Price":100000,"Tax":6000},
        {"Living Area":100,"Sewer":"Overhead","Bedrooms":4,"Last Sale Date":"4/4/2012", "Last Sale Price":100000,"Tax":8000},
        {"Living Area":100,"Sewer":"Overhead","Bedrooms":5,"Last Sale Date":"4/4/2013", "Last Sale Price":100000,"Tax":1000},
        {"Living Area":100,"Sewer":"Overhead","Bedrooms":2,"Last Sale Date":"4/4/2014", "Last Sale Price":100000,"Tax":52000}
    ]
    homes_df = pd.DataFrame(homesList, columns=["Living Area", "Bedrooms","Last Sale Date", "Last Sale Price"])

## Make a copy of a DataFrame

You'll end up with *two* DataFrames!

    my_copy = my_original_df.copy()

## Get the first rows of a data frame (head). (`tail` gets the last rows)

**By default, head gives the first 5 rows**

    my_awesome_df.head()

**Pass an integer to say how many rows you want**

    # Get the first 18 rows
    my_awesome_df.head(18)

## Display a statistical overview of the DataFrame or Series

### DataFrame

**Includes** count, mean, standard deviation, minimum, quartiles, and maximum *for each series*

    yummy_df.describe()

### Series

    cool_series.describe()

## Get columns from a DataFrame
### Get a single column

This gives you a *Series*

    fun_df["Roller Coasters"]

This gives you a *DataFrame* that has only one column/Series

    states_df[["capital"]]

### Get multiple columns - use double brackets

This gives you a *DataFrame*

    food_df[["carbs","fiber"]]

## Data Functions

### Average
* Of a Series - returns a *number*

        cities_df["population"].mean()

* Of a DataFrame - returns a *Series*

        nutrition_df.mean()

### Sum
* Of a Series - returns a *number*

        products_df["inventory"].sum()

* Of a DataFrame - returns a *Series*

        orders_df.sum()

### Standard Deviation
* Of a Series - returns a *number*

        infections_df["false positive"].std()

* Of a DataFrame - returns a *Series*

        tailor_measurements_df.mean()

### Unique values
* Of a Series - returns an array of type *numpy.ndarray*

        car_df["make"].unique()

* Get a count by unique value of the series - returns a *Series*
    
        car_df["make"].value_counts()

* Of a DataFrame - no such thing. You can only get unique values from a Series

## DataFrame and Series Arithmetic and Other Operations
* Perform arithmetic on a DataFrame - returns a *DataFrame*

    * divide every value in the DataFrame
    
            hundreds_revenues_df = revenues_df/100
    * or multiply every value in the DataFrame
    
            after_tax = invoice_df * 1.0925
    * or add a value to everything in the dataframe
    
            with_surchage_df = import_df + 22

* Perform arithmetic on a Series - returns a *Series*

    * divide every value in the Series
    
            run_miles = race_df["km"]/1.6
    * or multiply every value in the Series
    
            quarts = recipes["cups"] * 4
    * or subtract a value from everything in a series
    
            proposed_budget = current_expenditures["cost"] - 300

* Create a total column by summing all the columns

        pokemon_comparison_df["Total"] = pokemon_comparison_df.sum(axis=1)

## Sort

Sort by a column. By default, the sort is ascending

    strongest_pokemon_df = pokemon_comparison_df.sort_values(["Total"], ascending=False)

Sort by multiple columns

    family_and_generosity_df = happiness_df.sort_values(["Family", "Generosity"], ascending=False)

## Operate on columns of a DataFrame

### Get a list of columns

Returns an *Index*

    tv_shows_df.columns

### Rename one or several columns

* Parameter named columns is a dictionary with old names followed by new names

* Does not modify the original DataFrame, but returns a new one

        employees.rename(columns={"last_name":"Surname", "ssn":"ID"})

### Add a column (Series) to a DataFrame

* Note - the series must have the same number of items as there are rows in the DataFrame

        hr_df["$ Thousands"] = hr_df["Salary"]/1000

        # or

        customer_df["Rewards Points"] = initial_rewards_points

### Delete a column IN PLACE
**Note:** This mutates (changes) the actual dataframe

    del my_df["Dumb Field"]

### Count of fields within a dataframe that have values

Give a DataFrame with columns "LastName", "FirstName", "Employer", "City", State", "Amount" this counts the values in all columns

    people_df.count()

    OUTPUT:
    LastName     1776
    FirstName    1776
    Employer     1743
    City         1776
    State        1776
    Amount       1776
    dtype: int64

### Drop all rows that have any missing information (na="Not Available")

    people_df = people_df.dropna(how='any')
    people_df.count()

    OUTPUT:
    LastName     1743
    FirstName    1743
    Employer     1743
    City         1743
    State        1743
    Amount       1776
    dtype: int64

### Create a column that checks if another column has null values
The resulting column will contain True if the target column is null.

    people_df['Amount'].isnull()

    OUTPUT:
    0      False
    1      False
    2      False
    3      False
    4      False
        ...  
    1771    False
    1772    True
    1773    False
    1774    False
    1775    False

### Count all the nulls in a column

    people_df['Amount'].isnull().sum()

    OUTPUT:
    33


### Count all the duplicates
`duplicated()` creates a boolean like `isnull()` above

    people_df.duplicated().sum()

    OUTPUT:
    0

### Get the DataTypes for all columns

    people_df.dtypes

    OUTPUT:
    LastName      object
    FirstName     object
    Employer      object
    City          object
    State         object
    Amount        object
    dtype: object

### Change/Convert the datatype for the Amount column to numeric

    df['Amount'] = pd.to_numeric(df['Amount'])

*or* using python types like **float**, **long**, **double**, or **int**

    usa_ufo_df["duration (seconds)"] = usa_ufo_df["duration (seconds)"].astype("float")


## Update data

### Change values
Can be used to change many similar values to a single value. For example, correct misspellings

    cars_df["Make"] = cars_df["Make].replace({"Chevy":"Chevrolet","Chevie":"Chevrolet","Shevvy":"Chevrolet","Chevrolay":"Chevrolet","FORD":"Ford","Fjord":"Ford","Frod","Ford"})

### Replace all NaN or NA data with a value
If you want to make all your empties 0, or blank strings "None" or the like

    donors_df["Amount"] = donors_df["Amount"].fillna(0)

## Indexes

### ***The index is not a part of the data***

The index looks like a column, but it's not part of the data. It can be used as a row identifier, as columns have names, the index is like a row name. It is not part of the data.

### Change the index of a DataFrame

    df_with_new_index = original_df.set_index("last_name")

### Reset an index

This is useful *after sorting* to reset a numeric id to reflect the sort result.

    new_index_df = family_and_generosity_df.reset_index(drop=True)

## loc and iloc

### Use the loc indexing functionality to get columns and rows

Get the data in the row with index "Berry" for the columns "Phone Number" and "Time Zone"

    people_df.loc["Berry",["Phone Number", "Time Zone"]]

Get the data in the rows with index "Hudson" and "Morales" for columns "first_name" and "Phone Number"

    people_df.loc[["Hudson","Morales"],["first_name","Phone Number"]]

Get all data for rows "Richardson" and "Hudson" (retrieves all columns)

    people_df.loc[["Richardson","Hudson"]]

Same as above

    people_df.loc[["Richardson", "Hudson"],:]

Get all rows for the column "Phone Number" - Returns a *Series* if there are no brackets on the column name

    people_df.loc[:,"Phone Number"]

Get all rows for the column "Phone Number" - Returns a *DataFrame* if there are brackets on the column name

    people_df.loc[:,["Phone Number"]]

Get all rows for the columns "Phone Number" and "Time zone" - Returns a *DataFrame*

    people_df.loc[:,["Phone Number", "Time zone"]]

### Use conditionals with loc to select certain rows

Get all columns for records where the first name is "Billy

    only_billys = df.loc[df["first_name"] == "Billy", :]

With logical operators and (&) and or (|)

    only_billy_and_peter = df.loc[(df["first_name"] == "Billy") | (df["first_name"] == "Peter"), :]

### Use the iloc indexing functionality to get columns and rows (numeric, zero-based)

Get the first 5 rows, and the 2nd and 3rd columns

    people_df.iloc[0:4,1:2]

Get rows 3-10, all columns

    people_df.iloc[2:9,:]

*or*

    people_df.iloc[2:9]

*or*

    people_df.iloc[2:9,]

*or* (assuming there are 5 columns)

    people_df.iloc[2:9,0:4]

Get columns 1-3 for all rows

    people_df.iloc[:,0:2]

*or* (assuming there are 1000 rows)

    people_df.iloc[0:999,0:2]

## Grouping within a DataFrame

### Group by a single column

This results in a *DataFrameGroupBy* object

    grouped_usa_df = usa_ufo_df.groupby(['state'])

This object is pretty useless until you perform some operation (like sum or count) on it

*`count`* gives a count of all columns

    grouped_usa_df.count()


*`sum`* gives a sum of all columns

    grouped_usa_df.sum()

You can select a single column and get the sum of it

    grouped_usa_df["population"].sum()


## Merging DataFrames

Merging DataFrames is like performing SQL joins. There are inner, left outer, right outer, and (full) outer joins.

    books_df = pd.DataFrame([{"title":"Two Cities","authorid":1},
                        {"title":"Freakonomics","authorid":2},
                        {"title":"Ramona the Brave","authorid":3},
                        {"title":"A Christmas Carol","authorid":1}])

    authors_df = pd.DataFrame([{"name":"Dickens","authorid":1},
                          {"name":"Cleary", "authorid":3},
                          {"name":"Heminghway","authorid":4}])


### Inner Join

    inner_1 = pd.merge(books_df, authors_df, on="authorid")

*or*

    inner_2 = books_df.merge(authors_df, on="authorid")


### (Full) Outer Join

Below: Some books have author info. Some authors have no books.

    outer_1 = pd.merge(books_df, authors_df, on="authorid", how="outer")

*or*

    outer_2 = books_df.merge(authors_df, on="authorid", how="outer")

### Left Outer Join

Only include null/na values from the first dataframe referenced

    left_outer_1 = pd.merge(books_df, authors_df, on="authorid", how="left")

*or*

    left_outer_2 = books_df.merge(authors_df, on="authorid", how="left")

### Right Outer Join

Only include null/na values from the first dataframe referenced

    right_outer_1 = pd.merge(books_df, authors_df, on="authorid", how="right")

*or*

    right_outer_2 = books_df.merge(authors_df, on="authorid", how="right")

### Append suffixes to merged dataframe

If both dataframes have columns with the same names that are not being used in the "on" parameter, you can append a suffix to all columns

    merged_df = hospital_df.merge(clinic_df, on="hospitalid", suffixes=("Hosp","Clin"))


## Binning

When there are many discrete values in a dataset, it's nice to be able to create groups that are easier to understand

    stocks_df = pd.DataFrame([
        {"ticker":"AAPL", "name":"Apple", "Market Cap": 2018000},
        {"ticker":"AMZN", "name":"Amazon", "Market Cap": 1640000},
        {"ticker":"TSLA", "name":"Tesla", "Market Cap": 325859 },
        {"ticker":"BMY", "name":"Brisol Myers Squibb", "Market Cap": 133410 },
        {"ticker":"JPM", "name":"JP Morgan Chase", "Market Cap": 307183},
        {"ticker":"LUV", "name":"Southwest Airlines", "Market Cap": 23117},
        {"ticker":"MCD", "name":"McDonalds", "Market Cap": 161275 },
        {"ticker":"WORK", "name":"Slack", "Market Cap": 13875},
        {"ticker":"CVX", "name":"Chevron", "Market Cap": 149981},
        {"ticker":"DG", "name":"Dollar General", "Market Cap": 49127}
    ])

    # float('inf') is a representation of ininity
    size_bins = [0, 1000, 10000, 100000, 500000, float('inf')]

    # bin_names must have one fewer value than the bin edges above
    size_names = ["Tiny", "Small", "Medium", "Big", "Huge"]

    # Create a new column using cut (returns a *Series*)
    stocks_df["Size"] = pd.cut(stocks_df["Market Cap"], size_bins, labels=size_names)

## Changing Output Format (Mapping?)

To change the output representation of numbers, you can use the `map` function on a Series. Doing so will change the data type from numeric to object, so only do this when you're done performing arithmetic on the data.

    tax_lots_df = pd.DataFrame([
        {"ticker":"AAPL","price":350.3,"shares":1250,"change":1.2345},
        {"ticker":"AMZN","price":1800.55,"shares":500,"change":0.6322},
        {"ticker":"FB","price":301.09,"shares":1500,"change":-0.0912},
        {"ticker":"GOOG","price":1224.82,"shares":825,"change":0.261},
        {"ticker":"NFLX","price":501.18,"shares":1000,"change":0.0505},
        {"ticker":"LUV", "price":31.22, "shares":2500000,"change":0.2111}
    ])

    # Change the price to USD
    tax_lots_df["price"] = tax_lots_df["price"].map("${:.2f}".format)

    # Comma delineate thousands in the share count
    tax_lots_df["shares"] = tax_lots_df["shares"].map("{:,}".format)

    # Multiply change by 100, report it as a percentage with single decimal precision
    tax_lots_df["%change"] = (tax_lots_df["%change"]*100).map("{:.1f}%".format)





















...

...

...

# My Only Friend