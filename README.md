# jtc-market-sentiments
IMPT: I shifted the files here and there so make sure the file path is accurate, all the file paths are assumed to be in the same directory as the Jupyter notebook

## 1. data_collection.ipynb
OUTPUTS "urls.xlsx"

- Essentially extracts URLs using the specified google search item by the time period 
- Cleans the URLs by removing duplicates and filtering for those under the 5 news media source
- Scrapes the dates when the article is posted

OUTPUTS "texts.xlsx"
- Scrapes the texts from the article using the URLs
