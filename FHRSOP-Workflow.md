## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [`__init__`](#init)
3. [`scrape`](#scrape)
4. [`preprocess`](#preprocess)
5. [`prepare`](#prepare)
6. [`sqlize`](#sqlize)
7. [Updating and Modifying](#updating)
8. [Potential Improvements](#improvements)

## Description <a name="description"></a>

The FHRSOP workflow has been implemented using the `FHRSOP` class that has the following member functions:
1. `__init__` loads the input file _parameters.yml_
2. `scrape` generates scraped data _site-postcode.json_ in `data/raw`
3. `preprocess` generates preprocessed scraped data _site-postcode.csv_ in `data/interim`
4. `prepare` generates proprocessed user-editable data _*dbpart.csv_ in `data/interim`
5. `sqlize` generates SQL database in _*.db_ in `data/processed`

The flowchart summarizes this workflow, which is described in more detail below.

![Workflow_R_flowchart](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Workflow_R_flowchart.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## `__init__` <a name="init"></a>

The class constructor takes in the path of _parameters.yml_ as an input argument, and stores the parameters as an attribute `self._parameters` of type dict.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## `scrape` <a name="scrape"></a>

The `scrape` member function initializes a Scrapy project, then uses the parameters supplied by the user in _parameters.yml_ to instruct all `Spider` classes contained in a user-supplied source directory to begin crawling. Each of these crawl jobs generates a .json file in `data/raw/` of the form _site-postcode.json_.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## `preprocess` <a name="preprocess"></a>

The `preprocess` member function preprocesses each .json file produced by the `scrape` member function, and generates a .csv file in `data/interim/` of the form _site-postcode.csv_. This member function makes use of the `preprocess_jsonfile` utility function defined in _FHRSOP.py_, which in turn instantiates subclasses of the `Preprocessor` class defined in `src/features` to implement the actual preprocessing.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## `prepare` <a name="prepare"></a>

The `prepare` member function preprocesses each user-editable .csv file in `data/raw/` of the form _*_editable.csv_, and generates a .csv file in `data/interim/` of the form _*dbpart.csv_. This member function makes use of the _preparePlatforms.py_ and _prepareBusinessTypes.py_ Python scripts in `src/features/` to implement the actual preparation.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## `sqlize` <a name="sqlize"></a>

The `sqlize` member function uses the .csv files in `data/interim/` and the FHRS API using the `FHRSWrapper` member functions, and generates a SQL database in `data/processed/` of the form _*.db_. This member function instantiates the `SqliteCreator` class defined in `src/features` to implement the actual SQL database creation.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## Updating and Modifying <a name="updating"></a>

Each of these member functions can be modified directly in _FHRSOP.py_, or overridden by a subclass that inherits from the `FHRSOP` class. Additional components to the workflow can be added as a new member function of the `FHRSOP` class.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)

## Potential Improvements <a name="improvements"></a>

* **Formal interface**: the `FHRSOP` class is currently implemented as an informal interface with no class inheritance and where all member functions already contain their implementations. A further useful abstraction can be achieved by inheriting from `FHRSOP` and overriding its member functions with implementations that are defined in a separate subclass.

* **Scrapy API calls**: instead of running Scrapy via `scrapy crawl`, consider instead using the Scrapy API directly as described in the [Scrapy documentation](https://docs.scrapy.org/en/0.16/topics/practices.html).

* **Pipeline framework**: as this workflow expands, consider migrating it to sit on top of a pre-existing pipeline framework such as [DVC](https://dvc.org/doc/start/data-pipelines) or [Snakemake](https://snakemake.readthedocs.io/en/stable/).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow#table)