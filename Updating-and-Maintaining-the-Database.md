## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [`__init__`](#init)
3. [`create`](#create)
4. [`populatePlatforms`](#platforms)
5. [`insertListings`](#listings)
6. [`commit`](#commit)
7. [`close`](#close)
8. [Updating and Modifying](#updating)
8. [Potential Improvements](#improvements)

## Description <a name="description"></a>

SQLite database creation has been implemented using the `SqliteCreator` class that has the following member functions:
1. `__init__` creates a `Connection` object for a SQLite database, and a `Cursor` object to execute commands 
2. `create` creates all tables in the database using CREATE TABLE statements
3. `populatePlatforms` inserts rows into the Platforms, PlatformTypes, PlatformPlatformTypes, and BusinessTypes tables
4. `insertListings` inserts rows into the Establishments and Listings tables
5. `commit` commits the database
6. `close` closes the database

The schema summarizes this SQLite database creation, which is described in more detail below.

![DatabaseTechnical_Star](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/DatabaseTechnical_Star.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `__init__` <a name="init"></a>

The class constructor takes in the intended output path and instantiates a sqlite3 `Connection` class for the database, and a sqlite3 `Cursor` class which has an `execute` member function that allows for the execution of SQL statements within Python.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `create` <a name="create"></a>

The `create` member function creates the RawInput, Platforms, PlatformTypes, PlatformPlatformTypes, BusinessTypes, Establishments, and Listings tables with columns, primary keys, and foreign keys as drawn in the schema. Table creation is done with CREATE TABLE statements. Foreign key constraints are enforced with ON DELETE CASCADE and ON UPDATE CASCADE statements, which propagate the changes from the parent table to the child table when the parent key is updated or deleted.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `populatePlatforms` <a name="platforms"></a>

The `populatePlatforms` member function inserts rows into the Platforms, PlatformTypes, PlatformPlatformTypes, and BusinessTypes tables using _*dbpart.csv_ files in data/interim. 

This member function first inserts a row into the RawInput table for each input dbpart file. It then reads the contents of each file into a corresponding pandas dataframe, and uses that dataframe's `to_sql` member function to insert its contents into its corresponding table in the SQL database. This member function makes use of the `insertRawfile` utility member function to implement this operation, which in turn makes use of the `isRawinputPresent` utility member function to ensure that a file is only inserted into the RawInput table (and thus used to populate the corresponding table) only if it does not yet exist in the RawInput table (and thus does not yet exist as a table in the SQL database).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `insertListings` <a name="listings"></a>

The `insertListings` member function inserts rows in the Establishments and Listings table using scraped preprocessed .csv files stored in data/interim, cross-referenced with data obtained from the FHRS API using the `FHRSWrapper` class. 

For all postcodes that appear in the scraped files, the corresponding set of all outward codes, i.e. first part of the postcode, are passed to the `get_outward_postcode` member function of the `FHRSWrapper` class. This in turn calls `get_establishments` member function of the `FHRSWrapper` class which is a python wrapper around the FHRS API call of the same name, returning a collection of establishment details from the FSA's database based on the provided outward code. 

For each listing:
* If there is an exact match between the `FHRSID` scraped from the online platform and the `FHRSID` found in the FSA's database, then that listing is stored with the column value `IdentificationCertainty = 'confident: FHRSID from platform'`
* Else if there is an exact match between the `BusinessName` and PostCode scraped from the online platform matches the `BusinessName` and `PostCode` found in the FSA's database, then that listing is stored with the column value `IdentificationCertainty = 'confident: Postcode + full name'`
* Else if there is a fuzzy match between the `BusinessName` scraped from the online platform and the `BusinessName` found in the FSA's database, and an exact match between the `PostCode` scraped from the online platform matches the `PostCode` found in the FSA's database, then that listing is stored with the column value `IdentificationCertainty = 'medium: Postcode + partial name'`
* Else, that listing is stored with the column value `IdentificationCertainty = 'not found'`.

The set of establishments found during this operation are inserted as rows in the Establishments table, and the set of listings found with their `IdentificationCertainty` values are inserted as rows in the Listings table. Finally, this member function inserts a row into the RawInput table for each input scraped file that was used to populate these tables.

This member function makes use of the `isRawinputPresent` utility member function to ensure that a file is only inserted into the RawInput table (and thus used to populate the Establishments and Listings tables) only if it does not yet exist in the RawInput table (and thus has not yet been used to populate the Establishments and Listings tables).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `commit` <a name="commit"></a>

The `commit` member function calls a sqlite3 `Connection` class member function of the same name. 

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## `close` <a name="close"></a>

The `close` member function calls a sqlite3 `Connection` class member function of the same name. 

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## Updating and Modifying <a name="updating"></a>

Each of these member functions can be modified directly in _src/features/SqliteCreator.py_, or overridden by a subclass that inherits from the `SqliteCreator` class. Additional tables can be added to the database in the `create` member function.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)

## Potential Improvements <a name="improvements"></a>

* For this proof-of-concept, we designed a simple but fully-operational and -functional database that could serve as the basis for future development work. In this respect, we have suggestions on how the **database design** could be improved so it reduces data redundancy (i.e. data stored in two or more separate places) and, by means of doing so, efficiency is increased in both storage and data flow terms (i.e. if data is stored in one place only, updates to these data do not occur in multiple instances, which require more processing power). To do this, we propose to switch to a relational database snowflake design like the one pictured below. Implementing this alternative design should not pose too many technical difficulties and would also support further future modifications or extensions.

![DatabaseTechnical_Snowflake](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/DatabaseTechnical_Snowflake.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Updating-and-Maintaining-the-Database#table)