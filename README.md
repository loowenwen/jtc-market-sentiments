# jtc-market-sentiments
IMPT: I tidied some of the files here and there so make sure the file path is accurate, all the file paths are assumed to be in the same directory as the Jupyter notebook in the Python codes but that may not be the case. For example, I have moved the JSONL files into new folders, so the code would return code error if you run the code directly without modification

## 1. data_collection.ipynb
OUTPUTS "urls.xlsx"

- Essentially extracts URLs using the specified google search item by the time period 
- Cleans the URLs by removing duplicates and filtering for those under the 5 news media sources
- Scrapes the dates from the article using the URLs

OUTPUTS "texts.xlsx"
- Scrapes the texts from the article using the URLs

## 2. data_cleaning.ipynb
OUTPUTS "texts_cleaned.xlsx"

- If you run the code chunks individually, the cleaned texts is saved to "data_cleaning.xlsx" first, so can look at the genereted file to see if the cleaning is done accurately, or to your preference
- "texts_cleaned.xlsx" contains the cleaned texts that would be updated as the code runs because...
    -  I filtered the dataframe by the domain (source) to clean the texts separately
    - However, when saving to the "texts_cleaned.xlsx" the dataframe will contain the cleaned texts as well as the uncleaned ones, until all are cleaned
- For EdgeProp sources, I realised that there were many URLs that failed the text scraping initially, so I tried to rescrape the texts for the failed URLs
    - I explored rescraping the text twice...
        - But, not all URLs managed to be scraped successfully, so I actually just deleted those that has the output "failed to retrieve content"
        - Maybe can explore manual copy paste but I did not try that method as the number of articles were already sufficient to do the study on