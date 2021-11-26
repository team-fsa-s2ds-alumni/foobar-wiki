## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [Ecosystem](#ecosystem)
    1. [Schematic](#schematic)
    2. [Platforms Typology](#typology)
    3. [Platforms](#platforms)
3. [Restaurants](#restaurants)
4. [Social](#social)

## Description <a name="description"></a>

FHRSOP outputs a dashboard, an interactive tool to navigate the landscape of online food platforms (see the [FRHSOP Wiki](https://github.com/S2DSLondon/Aug20_FSA/wiki) for technical details). The dashboard is divided into three main areas: Ecosystem, Restaurants, and Social. 

![Dashboard_Home](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_Home.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)

## Ecosystem <a name="ecosystem"></a>

The Ecosystem tab consists of three subtabs with interrelated contents: 

### **Schematic** <a name="schematic"></a>

Describes in a visual manner all the steps from food production to food consumption. **Different food ecosystem schemas** are provided: 
* Classic food ecosystem
* Simplified and Complex current food ecosystems, which incorporate the observations derived from our exploration of today’s food landscape
* A series of example paths for specific food products which aim to clarify the complexities of the current food ecosystem
> * E.g. Some industrially farmed tomatoes could end at the hands of the consumer if he/she buys them directly in a supermarket 
> * E.g. Or, alternatively, through an intermediary like Amazon Fresh

![Dashboard_EcosystemExample](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_EcosystemExample.png)

You can explore these different schematics using the **filters** of the left-hand menu. 

![Dashboard_EcosystemFilters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_EcosystemFilters.png)

In addition, by clicking at each ecosystem point or node, you can obtain **further information** on: 
* The different specific food agents concerning that node (e.g. ordering & logistics and marketplaces in no-handling intermediaries) 
* The related food platforms that have been analysed to date and are included in our database (e.g. JustEat, Kukd, Facebook Marketplace, etc.).

![Dashboard_EcosystemNodes](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_EcosystemNodes.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)

### **Platforms Typology** <a name="typology"></a>

The different food agents depicted in the Schematic reflect a new online food typology map that we have devised from our investigation on the current food ecosystem and the insights of the FSA. 

In this tab, you can find the **descriptions and examples of the different platform typologies**, which consist of combinations of three subcategories:

* **Food Types**: Groceries or Raw Foods, Takeaway/Cooked Meals, and Food Events/Experiences (they include everything from caterings to supper clubs or food trucks).

* **Service Types**: Vendors, Intermediaries, and Informative (which include, for example, blogs that do restaurant reviews, webs that list restaurants, social media accounts on food events, etc.).

* **Business Model Types**: which vary depending on the different combinations of the previous subcategories (e.g. recipe boxes, supermarkets, ordering & logistics, etc.).

![Dashboard_TypologyTypes](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_TypologyTypes.png)

You can use the **filters** on the left-hand menu to explore the different category and subcategory types.

![Dashboard_TypologyFilters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_TypologyFilters.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)

### **Platforms** <a name="platforms"></a>

In the Platforms tab, you can find **general information** on the platforms analysed and categorised to date: 
* Platform name
* Platform URL
* Platform type

![Dashboard_PlatformsPlatforms](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_PlatformsPlatforms.png)

You can also see **specific information concerning food-related and web-scraping technical aspects** that are of interest to the FSA by clicking the button 'More Info' on each platform:
* Business Name Available (for platforms listing establishments)
* Business Address Available (for platforms listing establishments)
* FHRS [score] Shown or not (for platforms listing establishments)
* FHRS required on registration (for platforms listing establishments)
* Scraping Restrictions (i.e. platform's direct policy on scraping or general data access terms)
* Scraping Restrictions URL (if available)
* Official API URL (if available)
* Robots URL (if available)
* Robots Allows Search (if available)
* Javascript (whether the platform uses javascript or not - important variable for scraping as javascript requires specific methods; see the [Scraping](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping) technical guide) 
* [Records] Last Updated date
* [Records] Last Updated By who

![Dashboard_PlatformsMoreInfo](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_PlatformsMoreInfo.png)

The **filters** on the left-hand menu allow you to return different platform listings choosing from the different platform typologies described above.

![Dashboard_PlatformsFilters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_PlatformsFilters.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)

## Restaurants <a name="restaurants"></a>

In the Restaurants tab, you can find a list of the Food Business Operators (FBOs) or establishments obtained from the different platforms analysed to date with our built-in web scraping tool and stored in our database.

The summary box lets you explore a series of **descriptive statistics regarding the FBOs**: 
* No. of FBOs
* No. of platforms analysed
* No. of Local Authorities covered
* % of FBOs with low FHRSH or food hygiene rating scores (<3)
* Date of the most recent rating, Date of the oldest rating
* % of unidentified FBOs (these correspond to establishments retrieved from the platforms scrapings that have not been able to match with the information obtained from the FSA API) 
* % of unrated FBOs (FBOs which are awaiting to receive a score or are exempt for any reason) 

![Dashboard_RestaurantsSummary](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_RestaurantsSummary.png)

Two graphs additionally show the **distribution of the different FHRS scores for the FBOs listed and** the distribution of FHRS scores with respect to **when these FBOs were last inspected**. 

![Dashboard_RestaurantsGraphs](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_RestaurantsGraphs.png)

The **filters** on the left-hand menu allow you to explore these statistics by particular FHRS scores or scores ranges, by specific platform(s) analysed, and by Local Authority Number/Codes of interest - including or not unidentified and/or unrated FBOs.

![Dashboard_RestaurantsFilters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_RestaurantsFilters.png)

By playing with different filter searches, you can **answer questions** such as: 
* "Do one platform have more establishments with low/high ratings than another one?"
* "Which is the likelihood that an establishment last rated two years ago has a lower FHRS score than another one inspected much recently?"
* "Does this vary across platforms?"
* Etc.

Below the summary box, you can further observe the **list of FBOs** that are being analysed in each filter search and a series of extra information fields: 
* Business Name
* Business Address
* FHRSID (FSA's FBO identification)
* Local Authority Name
* Local Authority Code
* FHRS Rating Value
* Rating Date

![Dashboard_RestaurantsListings](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_RestaurantsListings.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)

## Social <a name="social"></a>

Lastly, the Social tab focuses on the contents retrieved from **social marketplaces** like Facebook Marketplace. These sort of platforms present a very different business model typology as they are characterised for enabling user-to-user selling and buying of food products and thus the establishment concept does not vastly apply nor, consequently, sellers are commonly registered in FSA records.

Given the particularities of these sites, the big challenge here was trying to **depict the landscape of online marketplaces**: i.e. Which food products are users selling? Which food products are users looking for?

To this, we conducted a series of specific food-related keywords searches (e.g. "home food london" or "ready meal London") with our web scraping tool extracting the information of the different post headings and descriptions. Then, we adopted natural language processing (NLP) techniques to analyse the linguistic terms contained therein.

The results of these word analyses can be observed in two visualisations: 

* **Wordcloud**: It has two dimensions — size and color. The size corresponds to the frequency of the words. Words in blue are food-related terms, and words in black are non-food related terms. The decision of whether a word is a food term or not is by looking up a dictionary (see the [technical guide](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP) for further details). There is a slider to the wordcloud which changes the number of top-most common words in the corpus. The total corpus size is printed in text.

![Dashboard_SocialWordcloud](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_SocialWordcloud.png)

* **Frequency Graph**: The graph reflects the count frequency of the top common words. The slider changes the number of words shown in the plot. The removed words correspond to all the removed words in the data processing stage ([technical guide](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP)). 

![Dashboard_SocialFrequency](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_SocialFrequency.png)

In addition, using the **filters** in the left-hand menu, you can generate *new* wordclouds of your interest. There are two filters: 

* **Search Term**: The search term specifies the keywords which keywords searches do you want to include in your visualisations. It contains a location  (e.g. "london") and a keyword term (e.g. "homemade food").

* **Location**: The location filter specifies the region where the _seller_ is located. It is worth noticing that Facebook's algorithm searches for a catching area extending 60 miles across different locations and will eventually also include results outside that search area. In other words, with the search "London homemade food" I could get products sold in e.g. Harrows. Therefore, this is an important aspect to take into account when conducting queries.

![Dashboard_SocialFilters](https://github.com/S2DSLondon/Aug20_FSA/blob/master/static/Dashboard_SocialFilters.png)

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#table)