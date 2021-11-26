## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [Installation](#installation)
    1. [Pre-requirements](#prerequirements)
    2. [Installation through Git](#git)
    3. [Installation through Docker](#docker)
3. [Generating the Database](#generating)
    1. [_parameters.yml_](#parameters)
    2. [Running FHRSOP](#running)
4. [Exploring the Database](#exploring)

## Description <a name="description"></a>

FHRSOP outputs a relational database which allows navigating the landscape of online food platforms. The database contains combined information retrieved directly from the platforms with our built-in web scraping tool and additional information on the same platforms by the FSA obtained through its API (see the [FHRS Wiki](https://github.com/S2DSLondon/Aug20_FSA/wiki) for technical details).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

## Installation <a name="installation"></a>

### Pre-requirements <a name="prerequirements"></a>

* FHRSOP operates through Conda, a free and open-source powerful package and environment manager for different coding languages ([install Conda on your device](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)). 
* If you want to install FHRSOP through A) Git, you will need to download Git, a free and open-source distributed version control system designed to handle everything from small to very large projects with speed and efficiency ([install Git on your device](https://git-scm.com/downloads)).
* If you want to install FHRSOP through b) Docker, you will need to download Docker Desktop, a tool designed to make it easier to create, deploy, and run applications by using containers or application packages ([install Docker Desktop on your device](https://www.docker.com/get-started)).
* To explore the database, you will need a database viewer such as DB Browser for SQLite ([install DB Browser for SQLite on your device)](https://sqlitebrowser.org/dl/) or any other that supports SQLite language.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

### Installation through Git <a name="git"></a>

1. Open your Git manager and run the following command:

```
git clone https://github.com/S2DSLondon/Aug20_FSA.git
```

_Note: If you work with MSWindows, please, make sure to clone the Git repository into a path on your hard drive that does not have spaces in the folder names (e.g. "C://Username/FSAProjects/" but NOT "C://Username/FSA Projects/")._

2. Open your Conda manager and run the following commands (command prompt in Windows or terminal window in macOs and Linux):

```
cd [path to git repository]/Aug20_FSA
conda env create -f environment.yml
conda activate fhrsop
pip install -e .
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

### Installation through Docker <a name="docker"></a>

* Build Docker image:

```
docker build -t fhrsop .
```

* Run Docker image:

```
docker run fhrsop
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

## Generating the Database <a name="generating"></a>

Prior to running any command on FHRSOP, the environment has to be activated:

```
conda activate fhrsop
conda env update -f environment.yml
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

### _parameters.yml_ <a name="parameters"></a>

This file contains the paths to the locations where different parts of the FHRSOP workflow will store and interact with the data and outputs of the project. 

The only section of _parameters.yml_ that should be customised by the user is the `scrapy` section. Here, users can edit:
* The `postcode` that will be used to scrape Deliveroo and Kukd with _DeliverooSpider.py_ and _KukdSpider.py_, respectively
* The `city`, `keywords`, number of maximum results to be returned (`max_n_results`), and `scroll_pause_time` parameters for _FBMktplaceSpider.py_
* The `date` to append to the output files from all spiders 

See further technical details on these parameters in the [Scraping](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping) guide.  

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

### Running FHRSOP <a name="running"></a>

```
python FHRSOP.py
```

This command will run the full FHRSOP pipeline (i.e. scraping, data preprocessing, and database generation) following the instructions from _parameters.yml_. Note that this can take ca. 1 hour if all three spiders are run. 

If you want to skip the scraping process (all or certain platforms), you can do it by commenting out (i.e. adding a # at the beginning of the line) the desired platforms (see the example below with Kukd scraping skipped out).

![Database_Parameters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Database_Parameters.png)

A database file (_fhrsop.db_) will be generated and stored in `/data/processed/` at the end of the pipeline. _fhrsop.db_ is also a requirement to use the [FHRS live Dashboard](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide).

_**Important! Every time you run FHRSOP with scraping turned on, be aware that the database will change, as the listings for each platform are not static.**_

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)

## Exploring the Database <a name="exploring"></a>

_Note: DB Browser - or any other preferred SQLite database viewers - offer multiple functionalities; here we just outline the very basic ones._

1. Open DB Browser.

2. Go to the `Browse Data` tab and select the table of interest in the `Table` box.

![Database_Browse](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Database_Browse.png)

3. Go to the `Execute SQL` tab to make specific search queries.

![Database_Query](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Database_Query.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Database-User-Guide#table)