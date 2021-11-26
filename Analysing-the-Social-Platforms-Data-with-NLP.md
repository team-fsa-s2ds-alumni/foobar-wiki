## Table of Contents <a name="table"></a>
1. [Description](#description) 
2. [Input Files](#input)
3. [Updating and Modifying](#updating)
4. [Potential Improvements](#improvements)

## Description <a name="description"></a>

The visualisations for **Social Platform Listing** can be found in the Social Tab on the Shiny dashboard. The visualisation aims to map out the landscape of the Facebook Marketplace by using specific or multiple food-related search terms.

There are two plots in the dashboard: one **wordcloud** with highlighted food-related words (in blue), and one **frequency graph** with word count frequency (see the [Social section of the Dashboard User Guide](https://github.com/S2DSLondon/Aug20_FSA/wiki/Dashboard-User-Guide#social) for more descriptive information).

All outputs rendered in the dahsboard are written in **R**.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP#table)

## Input Files <a name="input"></a>

* **Words Source**: All data for visualisation come from the database file _fhrsop.db_ saved in the data/processed folder. For the visualization herein, the text comes from the database table Listings filtered by partial-matching "Social" in the BusinessModelType field (Listings —> BusinessModelType —> Social). Currently, the data contains between 20 to 200 for each search term, since the spiders were set to return 200 posts, with the exception of "london, homemade food", with 499 posts. See table below for the number of posts by search term.
<center>

|ScrapingSearch       |Count(*)|
|---------------------|--------|
|london, cooked food  |200     |
|london, delicious    |198     |
|london, free food    |149     |
|london, fresh        |199     |
|london, homemade food|499     |
|london, meat         |200     |
|london, party food   |199     |
|london, pet food     |200     |
|london, raw food     |199     |
|london, ready meal   |20      |
|london, seafood      |199     |
|london, treat        |200     |
</center>

* **Food Dictionary**: The dictionary is an editable .csv file (_FoodDictionary_editable.csv_ in src/visualization/shiny) used for looking-up the food words when rendering the wordcloud. The original dictionary comes from Python package [nltk.corpus.wordnet 'food.n.02'](https://www.nltk.org/howto/wordnet.html), with additional food words manually added to the file.

* **Omitted Words**: The file is an editable csv file (_OmittedWords_editable.csv_ in src/visualization/shiny) containing a list of words that are removed from the visualisations. These are manually chosen non-food neutral words with high frequency. Without this omission list, the high frequency of these non-food words can potentially undermine the food-related words in the wordcloud.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP#table)

## Updating and Modifying <a name="updating"></a>

1. **Database to Dataframe**: Data are extracted from the database into a dataframe in R. Each row represents a marketplace post from the website. Texts within each database row are transformed into a row in the dataframe by concatenating the words in PlatformBusinessName and PlatformDescription from the Listings database table. <br></br>

2. **Dataframe to Word Tokens**: texts are read into ASCII and all special unicodes (e.g. emojis) are stripped. Texts within each row (i.e. each posting) are tokenised and vectorised, with the following standard transformations: lower case, number removal, punctuation removal, and stopword removal. Additional non-standard transformations are as following and in order:
> * **Removing custom words** from the list of _OmittedWords_editable.csv_ (in src/visualization/shiny), as well as from the complete list of (tokenised) **collective search terms** (e.g. "london,", "homemade", "food", "pet", "cooked"). The reason of removing all search terms, instead of only the ones relevant to the current filter (e.g. "london, cooked food"), reflects the assumption that all current search terms are relevant to one common interest — mapping food-related landscape. By sharing all the words to be removed, each individual wordcloud can be more readily compared to one another without changing its definition.
> * **Removing suffix 's'** at the end of each word: Instead of stemming, which results in a wordcloud with chopped-off words, to achieve a user-friendly visualisation, all 's' suffix are removed from the tokenised words. This has several advantages: a) it is quick to implement and b) the true word frequency for most nouns is generally preserved (with the exception of nouns ending with 'es', such as boxes).

> > Challenges: Before settling on removing the suffixes, we attempted to stem words and then to backward complete the stemmed words. However, this turned out to be a cumbersome process. To complete stemmed words, it was necessary to make a copy of the original corpus as a dictionary to look up in a loop (by completing the stemmed word to the most frequent word in the original corpus). This was proven to be extremely demanding on the memory and can make the visualisation completely unscalable.

3. **Word Tokens to Word Count**: Vectorised word tokens are transformed into a list of corpus (unique words) mapped onto a term-to-document (or word-to-post in our case) matrix, using the [tm package](https://cran.r-project.org/web/packages/tm/tm.pdf). The final word count is achieved by aggregating the count from each post (matrix column) and returning a count vector for the corpus.

> > Challenges: Before settling on the approach with the food-dictionary, we also explored the option of word-embedding using a simple two-layer neural network using the [text2vec package](https://cran.r-project.org/web/packages/text2vec/index.html). The advantage of NN with sufficient data is that, instead of having a cloud for different search term(s), a more comprehensive map can be achieved through the learning process. This can return a landscape where users can query specific terms to get the wordcloud landscape that is most relevant to the term (e.g. cooked+food - baked). However, with insufficient data, we could not achieve a wordcloud that told a story. We decided not to opt for the option for two reasons: a) users for the dashboard are not likely to be familiar with NN and, therefore, it may not be intuitive to the users what the wordcloud means; and b) there is insufficient data to generate word landscape using a neural network.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP#table)

## Potential Improvements <a name="improvements"></a>

* **Data**: Currently there are a total of 2,462 FB Marketplace posts in the database, obtained through different search terms. This is quite a limited amount of data to work with for more complex NLP models.

* **Scalability**: Since the visualisation is updated live on the dashboard, when the document size (i.e. number of posts) increases, there is a compromise on the speed of visualisation. Future work with large dataset may require decoupling the data processing part, such as the term-to-document matrix, as a module independent to the wordcloud rendering.

* **Word omission**: There are a lot of words that require manual removal. This is due to the fact that the wordcloud is based on word frequency and there are many common non-food terms in the corpus. The reason there is a "london," with a comma in the list is due to the fact that the search term "london, homemade" was tokenised using string split. This should be improved further by removing the comma in data processing.

* **keyword biases**: The wordcloud landscape relies entirely on which search term(s) are entered on Facebook Marketplace. This process is entirely biased as we do not really know what is the best proxy to get to the full picture. In addition, the posts are not manually tagged to their correct category (food post or non-food post), so it is entirely possible that many of the posts are not relevant to the research interest. Future works using manually labelled tags and supervised machine learning models are needed to correctly identify the relevant posts.

* **Interpretability**: There is a good degree of interpretability of the wordcloud and frequency chart as they are considered to be highly intuitive. However, since our wordcloud is purely descriptive, it should not be used as an analytic and predictive tool. This point should be clearly communicated with the stakeholders.

* **Location biases**: Many of the posts appear to be centred around East London. We are not sure whether this reflects to a higher activity in that region, to keyword biases, or to certain hidden, algorithmic factor from Facebook. The scraping of all but one search terms was done by a team member who lives in Chelmsford, London. The single exception of the "London, homemade food" was done by a team member residing in Cambridge.

* **Geographical Expansion**: As current searches are centred London and, through Facebook's algorithm, there is a catchment area of a 60-mile radius. There is a great deal of work needed to achieve geographical expansion. When searching on Facebook Marketplace, this radius definition can be manually changed in order to get a more centralised output. However, this parameter is not currently coded in our spiders. In addition, it is not a trivial concern that the posts may change according to the IP address. When the scraping was done in a virtual machine with an IP in Amsterdam, we were unable to get the London results despite having the correct URLs that contained our search terms. Thus, it would appear that different users may get different results.

* **Temporal Expansion**: We believe that this is probably quite relevant to the marketplace as there are many more home-based business activities since COVID-19; monitoring the platform through time may be relevant.

* **Neural Network-based wordcloud**: As mentioned in the previous sections, with enough data, there is a potential to achieve a comprehensive map of the food landscape. Specifically, with the neural network-based NLP, users can query specific terms (e.g. raw meat) and get a map for other commonly co-occurring words.

[Go to Top](https://github.com/S2DSLondon/Aug20_FSA/wiki/Analysing-the-Social-Platforms-Data-with-NLP#table)