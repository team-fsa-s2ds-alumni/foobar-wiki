## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [Updating and Modifying](#updating)
    1. [Adding/removing/modifying a platform in Platforms](#platform)
    2. [Adding/removing/modifying a platform’s column in Platforms](#column)
    3. [Adding/removing/modifying a platform type in Platforms Typology](#type)
    4. [Adding/removing/modifying a schema in Schematic](#schema)
3. [Potential Improvements](#improvements)

## Description <a name="description"></a>

FHRSOP outputs a Shiny dashboard which allows navigating the landscape of online food platforms. The dashboard is divided into three main areas: Ecosystem, Restaurants, and Social. This wiki page will address the Food Ecosystem – see [Generating and Maintaining the Database](https://github.com/S2DSLondon/Aug20_FSA/wiki/Generating-and-Maintaining-the-Database) and [Analysing the Marketplace Data](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Marketplace-Data-with-NLP), respectively, for technical documentation on the other areas. 

The Ecosystem tab consists of three subtabs with interrelated contents: 
* **Schematic**: Describes in a visual manner all the steps from food production to food consumption in different schematic representations (classic, current simplified, current complex, example paths). 
* **Platforms Typology**: Shows and describes the different food platform typologies reflected in the schematic.
* **Platforms**: Lists the platforms analysed so far, offering general information and specific details regarding food-related variables as well as technical details on scraping these platforms.

For further descriptive information about the ecosystem schematic, platforms, and platforms typologies, please, refer to [Dashboard User Guide](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)

## Updating and Modifying <a name="updating"></a>

As mentioned, the contents concerning these three areas are interrelated, so updating or modifying them can affect different layers of the workflow.

### Adding/removing/modifying a platform in Platforms <a name="platform"></a>

1. Open _Platforms_editable.csv_ (in data/raw) and add/remove/modify a platform filling all the fields correspondingly
2. Remove your local files in data/interim and your local copy of the _fhrsop.db_ database in data/processed 
3. Comment the scraping in _parameters.yml_ ([unless you want to scrap again](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow))
4. Run the first command in your Conda terminal to execute the FHRSOP and the second if you want to reload the dashboard again: 
```
FHRSOP.py
R -e “shiny::runApp(‘src/visualization/shiny/’)”
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)

### Adding/removing/modifying a platform’s column in Platforms <a name="column"></a>

1. Open _Platforms_editable.csv_ (in data/raw) and add/remove/modify the column inputting the right information for each platform accordingly
2. Open _preparePlatforms.py_ (in src/features) and accordingly add/remove/modify line 99
3. Open _SqliteCreator.py_ (in src/features) and add/remove/modify the corresponding lines in the Platforms table (starting at line 34)
4. Open _ecosystems_pages.R_ (in src/visualization/shiny/R) and add/remove/modify the corresponding lines (lines 412 to 478)
5. Remove your local files in data/interim and your local copy of the _fhrsop.db_ database in data/processed 
6. Comment the scraping in _parameters.yml_ ([unless you want to scrap again](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow))
7. Run the first command in your Conda terminal to execute the FHRSOP and the second if you want to reload the dashboard again: 
```
FHRSOP.py
R -e “shiny::runApp(‘src/visualization/shiny/’)”
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)

### Adding/removing/modifying a platform type in Platforms Typology <a name="type"></a>

1. Open _PlatformTypes_editable.csv_ (in data/raw) and add/remove/modify the platform type inputting the right information 
2. Open _preparePlatforms.py_ (in src/features) and accordingly add/remove/modify line 104
3. Open _SqliteCreator.py_ (in src/features) and accordingly add/remove/modify the corresponding lines in the Platforms Typology table (starting at line 52)
4. Open _ecosys_specifications.txt_ (in data/raw) and correspondingly modify the values in the column `ecosys_description` (see ‘Adding/removing/modifying a schema in [Ecosystem] Schematic’ below for further info)
6. Open _ecosys_nodes.txt_ (in data/raw) and correspondingly modify the values in the column `label` as well as all the values of all the `*_title columns` (see ‘Adding/removing/modifying a schema in [Ecosystem] Schematic’ below for further info)
7. Open _ecosystems_pages.R_ (in src/visualization/shiny/R) and add/remove/modify the corresponding lines (lines 15 to 30, 139 to 144, 161 to 168, 255, 274 to 288, 355-370, 409) 
8. Remove your local files in data/interim and your local copy of the _fhrsop.db_ database in data/processed 
9. Comment the scraping in _parameters.yml_ ([unless you want to scrap again](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow))
10. Run the first command in your Conda terminal to execute the FHRSOP and the second if you want to reload the dashboard again: 
```
FHRSOP.py
R -e “shiny::runApp(‘src/visualization/shiny/’)”
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)

### Adding/removing/modifying a schema in Schematic <a name="schema"></a>

1. Open _ecosys_specifications.txt_ (in data/raw) and correspondingly add/remove/modify the values affected in the different columns:
> * `ecosys_name` = name given to the schema
> * `ecosys_dropdown` = name of the schema for the dropdown menu
> * `ecosys_dropdown_tite` = name of the schema that will be displayed in the dropdown menu of the dashboard
> * `ecosys_description` = description of the ecosystem (path followed in case of example paths)
> * `nodecolor` = name of the linked node color columns of ecosys_nodes.txt
> * `title` = name of the linked node title columns of ecosys_nodes.txt
> * `edgecolor` = name of the linked edge color columns of ecosys_edges.txt
> * `width` = name of the linked edge width columns of ecosys_edges.txt
> * `fontcolor` = name of the linked edge color columns of ecosys_nodes.txt

2. Open _ecosys_nodes.txt_ (in data/raw) and correspondingly add/remove/modify the values affected in the different columns:
> * `id` = node id
> * `level` = node level in the schema
> * `label` = node label
> * `shape` = node shape (see visNetwork reference below)
> * `value` = node weight (see visNetwork reference below)
> * `x` = node x coordinate position
> * `y` = node y coordinate position
> * `color` = node color
> * `*_color` = node color (default for base color, highlighted for nodes that belong to a schema path example, deselected for nodes that want to be kept on the background on a schema path example, NA for nodes that want to be hidden)
> * `*_title` = node description for the nodes that belong to a schema (the rest, keep unfilled)
> * `*_fontcolor` = node font color (white if a node wants to be hidden, otherwise black)

3. Open _ecosys_edges.txt_ (in data/raw; the edges file specifies the arrows between nodes) and correspondingly add/remove/modify the values affected in the different columns:
> * `from` = id of the node the arrow starts
> * `to` = id of the node the arrow ends
> * `arrows` = type of arrow (see reference below) 
> * `dashes` = T (true) for finished foods and F (false) for raw foods
> * `length` = arrow length
> * `*_color` = color for each arrow (in 00C0EF – a type of light blue – for the arrows that belong to a schema, in grey for arrows not belonging to a schema but that want to be kept in the background, in white the ones that want to be hidden)
> * `*_width` = width for each arrow

4. Remove the local files in data/interim and your local copy of the _fhrsop.db_ database in data/processed 
5. Comment the scraping in _parameters.yml_ ([unless you want to scrap again](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow))
6. Run the first command in your Conda terminal to execute the FHRSOP and the second if you want to reload the dashboard again: 
```
FHRSOP.py
R -e “shiny::runApp(‘src/visualization/shiny/’)”
```

See [Generating and Maintaining the Dashboard](https://github.com/S2DSLondon/Aug20_FSA/wiki/Generating-and-Maintaining-the-Dashboard) and [visNetwork](https://datastorm-open.github.io/visNetwork/) for further information on implementing the ecosystem schematics.

_General Note: .csv and .txt (i.e., tab-separated values in this case) files can be edited in Excel or other editors, but, please, make sure that the comma and tab separations, respectively, are properly kept when saving the updated/modified files._

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)

## Potential Improvements <a name="improvements"></a>

* The FSA is currently in the process of reconceptualising the online food market landscape. For this reason, and because the need to highlight one or another aspect of the food ecosystem might be subjected to constant changes in the upcoming times, it is difficult to make recommendations with respect to **how the typology could be improved**. However, from our brief research and talks with several people from the FSA implicated in this area, we could suggest adding a further subcategory that accounted for whether food is handled or not. At the moment, the intermediaries subcategories have this distinction, but it might be worth thinking about to incorporate it in a global way, as this has major implications regarding FHRS policies. 

* On the technical side, it would be very interesting and useful to **automatise the retrieval of information regarding the food typology map from a single source**. However, due to the highly-specific and -customised inputs needed to populate the ecosystem schematic and the platforms typology table, it might be a very difficult enterprise.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Preparing-the-Food-Ecosystem,-Food-Platform-Typology,-and-Food-Platforms#table)