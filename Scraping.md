## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [Polite Scraping](#scraping)
3. [Spiders in FHRSOP](#spiders)
    1. [_KukdSpider.py_](#kukd)
    2. [_DeliverooSpider.py_](#deliveroo)
    2. [_FBMktplaceSpider.py_](#fb)
4. [Updating and Modifying](#updating)
5. [Potential Improvements](#improvements)

## Description <a name="description"></a>

FHRSOP can access the websites of online Food Business Operators (FBOs) and automatically retrieve data (this is known as "scraping"). FHRSOP is written in Python and relies heavily on [Scrapy](https://scrapy.org/), a popular open-source framework for web scraping. A good starting point is to check out this [Scrapy tutorial](https://docs.scrapy.org/en/latest/intro/tutorial.html).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

## Polite scraping <a name="scraping"></a>

Some websites put measures in place to prevent automated access to their information. These can vary in their degree of sophistication but may ultimately result in your IP being banned from accessing the site.

* The first check when planning to scrape a website should be to look at their [**robots.txt**](https://support.google.com/webmasters/answer/6062608?hl=en). This document contains information on the website's policy towards automated robots and may block access to all or certain parts of the website. We have compiled information on scraping policies for 79 online FBOs ([Platforms](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#platforms)). FHRSOP currently ignores the restrictions listed on robots.txt because gathering information from Facebook Marketplace is otherwise not possible. This option can be turned off (i.e. FHRSOP will not ignore robots.txt) by modifying _FHRSOP.py_ as follows: 
the line
```
command = ['scrapy', 'crawl', spider, '--set=ROBOTSTXT_OBEY="False"', '--set=DOWNLOAD_DELAY=1.0']
```
should be replaced by
```
command = ['scrapy', 'crawl', spider, '--set=ROBOTSTXT_OBEY="True"', '--set=DOWNLOAD_DELAY=1.0']
```

* Another relevant parameter is **`DOWNLOAD_DELAY`**. This is the waiting time in seconds between consecutive downloads of pages in the same site. Larger values of this parameter will result in smaller load to the servers and may avoid getting blocked. At the moment FHRSOP is set to `DOWNLOAD_DELAY=1.0` to speed up our tests. However, Scrapy recommends to set it to `2.0` in its ["Avoid Getting Banned" section](https://docs.scrapy.org/en/latest/topics/practices.html#avoiding-getting-banned). This can be done by modifying _FHRSOP.py_ to set ROBOTSTXT_OBEY to True in the following line:
```
command = ['scrapy', 'crawl', spider, '--set=ROBOTSTXT_OBEY="False"', '--set=DOWNLOAD_DELAY=1.0']
```

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

## Spiders in FHRSOP <a name="spiders"></a>

Spiders are self-contained classes in scrapy that are used to extract information from a website. Typically spiders are run through the command line:
```
scrapy crawl <nameofthespider> -o <outputfile> -a <argument1=argument1>
```

In the current version of FHRSOP, we have scraped the websites of three online FBOs ([Deliveroo](http://www.deliveroo.co.uk/), [Kukd](http://www.kukd.co.uk/), and [Facebook Marketplace](http://www.facebook.com/marketplace/)). These websites all have very different complexities and ways of storing the information that we are interested in. Thus, FHRSOP currently has three spiders (_DeliverooSpider.py_, _KukdSpider.py_, and _FBMktplaceSpider.py_) that each deal with a different site. FHRSOP spiders have two modes that are selected with `-a mode`: "run" (the standard one, to analyse a website) and "test" (to perform unit tests with a shorter version of the protocol). All FHRSOP spiders output results in a JSON format. 

FHRSOP currently gathers all the restaurants that deliver to [E16AW](https://www.google.co.uk/maps/place/Shoreditch,+London+E1+6AW/@51.5239313,-0.0780072,17z/data=!3m1!4b1!4m5!3m4!1s0x48761cb7356913ef:0x70cfa30b8b6cdc43!8m2!3d51.5239362!4d-0.0758712) for Deliveroo and Kukd, while for Facebook Marketplace it searches items in London.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

### _KukdSpider.py_ <a name="kukd"></a>

_KukdSpider.py_ is the simplest spider in FHRSOP. All the relevant information on each restaurant (business name, address, cuisine type) can be extracted from a javascript instance that is loaded when a search for a specific postcode is carried out. This means that there is no need to scrape the individual restaurant sites and scraping here is very fast.
As arguments, _KukdSpider.py_ takes:
* `mode` = run/test (as all spiders on FHRSOP)
* `postcode`= e.g. E16AW (it searches for all the restaurants that deliver to a particular postcode)

A typical run would be:
```
scrapy crawl kukd -o kukd_E16AW.json -a mode="run" -a postcode="E16AW"
```

The output file contains the following fields: `heading` (the name of the restaurant); `postcode` (the postcode of the restaurant); `address_line1`; `address_line2`; `address_line3`; `reviews_count` (the number of reviews given by users on the platform); `cuisine_types` (the tags for the restaurant on the platform); `reviews_average` (the score given by users on the platform); `has_halal_food` (true or false); `search_term1` (the postcode that was used to search); and `time` (the time of the search).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

### _DeliverooSpider.py_ <a name="deliveroo"></a>

_DeliverooSpider.py_ is slightly more complicated than _KukdSpider.py_. It first fetches the links for the individual pages of all the restaurants when a search for a specific postcode is carried out. Then it accesses the individual sites and gathers all the relevant information (including FHRS information from a javascript instance - see [below](LINK) for more details). The need to access individual restaurant sites means that _DeliverooSpider.py_ runs may take up to 30 minutes (depending on the number of restaurants in the search and on the `DOWNLOAD_DELAY` parameter).

As arguments, DeliverooSpider takes:
* `mode` = run/test (as all spiders on FHRSOP)
* `postcode` = e.g. E16AW (it searches for all the restaurants that deliver to a particular postcode)

A typical run would be: 
```
scrapy crawl deliveroo -o deliveroo_E16AW.json -a mode="run" -a postcode="E16AW"
```

The output file contains the following fields: `heading` (the name of the restaurant); `rating` (the score given by users on the platform); `description` (the description of the restaurant); `fsa_id` (the ID assigned in the FSA API, if it exists); `fhrs` (the FHRS rating displayed on the platform, if it exists); `extras` (free text - contains information on types of cuisine); `search_term1` (the postcode that was used to search); and `time` (the time of the search).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

### _FBMktplaceSpider.py_ <a name="fb"></a>

FBMktplaceSpider is the most complex spider in FHRSOP. This is because Facebook Marketplace has infinite scrolling and because we have to limit the number of maximum results retrieved.

* **Infinite scrolling** is built into many sites and allows for new content to be loaded continuously as you scroll down the page. Scrapy cannot easily deal with websites that have this feature. We have created a method (`get_item_urls_selenium`) based on [Selenium](https://selenium-python.readthedocs.io/), which is another Python module for web-scraping that focuses on automating web browser interaction. `get_item_urls_selenium` mimics a user scrolling down to the bottom of a page by calculating the height of the bottom of the page every n seconds (n is determined with the parameter `SCROLL_PAUSE_TIME`). If the height of the page has grown (i.e. the new height of the page is bigger than the older one), it means that new content has been loaded. Since Facebook Marketplace loads new content in an asynchronous way, larger values of `SCROLL_PAUSE_TIME` (a minimum of `2.0` - `5.0` even better) have to be picked to ensure that all items in a search are retrieved (this means that it is also probably a good idea to run multiple replicates of FBMktplaceSpider).

* We have limited the **number of results retrieved** when scraping Facebook Marketplace. This is to prevent blocking from Facebook, who doesn't allow "engaging in Automated Data Collection without Facebook's express written permission". The maximum number of results retrieved is controlled with the parameter `MAX_N_RESULTS`.

After fetching the links for the individual items when a search for a particular keyword(s) in a particular location is carried out, _FBMktplaceSpider.py_ opens the sites of the individual items and extracts the item title, location, and item description from a javascript instance. No user name or precise location are collected to comply with Data Protection regulations. 

It is important to note that Facebook eventually shows results from outside the search area (which is itself 60 miles wide around the target city) when scrolling down. The `location` field in the output field of _FBMktplaceSpider.py_ can be used to filter out unwanted results.

As arguments, FBMktplaceSpider takes:
* `mode` = run/test (as all spiders on FHRSOP)
* `city` = e.g. london (it searches for items in the London marketplace)
* `keywords` = e.g. "homemade food" (the keywords for the marketplace search)
* `max_n_results` = e.g. 200 (the maximum number of items to be retrieved, see above)
* `scroll_pause_time` = e.g. 2.0 (the delay between scrolling tries, see above)

A typical run would be:
```
scrapy crawl fbmktplace --set=ROBOTSTXT_OBEY='False' -o fbmktplace_london_homemadefood.json -a city='london' -a keywords='homemade food' -a max_n_results=200 -a scroll_pause_time=2.0
```

The output file contains the following fields: `heading` (the title of the item on the platform); `description` (the seller's description for the item); `location` (a broad location term); `search_term1` (the city that was used to search); `search_term1` (the keyword(s) that was used to search); and `time` (the time of the search).

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

## Updating and Modifying <a name="updating"></a>

The following steps can be useful when adding a new website to be scraped with FHRSOP:

1. **Check the site's robots.txt** and the Terms and Conditions for scraping restrictions. In the robots.txt, it is important to ensure that the path to the information that will be scraped is not under `Disallow`. We have compiled this information for 79 online FBOs in a spreadsheet ([Platforms](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#platforms)).

2. **Find the information to be scraped** in the HTML source of the website. The easiest way of doing this is by inspecting the web elements with the browser (e.g. using the Developer Tools in Google Chrome). It is also important to check the contents of any javascript instances that are run on the page because they often store useful information. 

3. **Write the spider to select the target information**. Once the target information is located within the HTML source, CSS or Xpath [selectors](https://docs.scrapy.org/en/latest/topics/selectors.html) can be used in a spider to extract it. Both types of selectors can be grabbed from the HTML code when Inspecting Elements by `right-click->Copy` on any part of the website.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)

## Potential Improvements <a name="improvements"></a>

* **Running Spiders from script**: FHRSOP spiders are currently set to run using `scrapy crawl`. They could also be run from a [script](https://docs.scrapy.org/en/0.16/topics/practices.html), but this would take substantial restructuring and rewriting.

* **FHRS information on Deliveroo**: Deliveroo only displays FHRS (with an image that is obtained from the FSA API) when users are logged in to their account on the site. FHRS is not visible when scraping with scrapy or in incognito mode - the only information that can be obtained then is the FSA ID for the business (which FHRSOP uses at a later step to query the FSA API). _DeliverooSpider.py_ in FHRSOP can also grab the text associated with the 
FHRS image if it exists. This could be useful should Deliveroo switch to showing FHRS info in incognito mode or should FHRSOP users choose to log in when scraping websites.

* **Getting names of business owners from the HTML source in Deliveroo**: the names of business owners can be found in the HTML code in Deliveroo. We have not included this functionality, amongst other things, to comply with Data Protection Regulations. However, this could be useful to resolve some situations where information from a restaurant does not have an exact match on the FSA API.

* **There are some "weak spots" in the code of the spiders**: these are the points that are most likely to break if the HTML source of the website changes. For example, lots of the HTML classes within the Facebook Marketplace code are named using very complex and long strings of letters. _FBMktplaceSpider.py_ accesses some of these classes using these complex strings to get information that they store. If these strings were slightly modified by Facebook, the spiders will no longer be functional.

* **Searching multiple postcodes**: currently searches in Deliveroo and Kukd are done with a single postcode, taken from the [_parameters.yml_](https://github.com/S2DSLondon/Aug20_FSA/wiki/FHRSOP-Workflow) file. This file could be easily modified to loop through a list of postcodes. However, two close postcodes will have very similar lists of restaurants delivering to them, so redundancy should be taken care of if searching multiple postcodes in the same area.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Scraping#table)