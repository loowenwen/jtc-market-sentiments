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
        - Maybe can explore manual copy paste but I did not try that method as the number of articles were sufficient to do the study on

## 3. chatgpt_batch.ipynb
OUTPUTS JSONL FILES FOR CHATGPT BATCH API

- Under the Generating JSONL File section

FEEDS JSONL FILES INTO CHATGPT API

- Link to access the ChatGPT Batch API: https://platform.openai.com/batches
    - Can keep track if the batches are done, and download the output files
    - IMPT: ChatGPT API has an enqueued token limit of 90,000 tokens, meaning, in some cases might have to separate the texts in the same sheet into smaller files, I did not face issues for the articles but I faced this issue for the consultancy reports as the word count for the reports are too high. 

JSONL FILES

- All the created JSONL files meant to be inputted into ChatGPT Batch API are found in chatgpt > jsonl
- However, for the downloaded results (which are in JSONL file formant) are split accordingly to the time frame i.e. 2010_2014

OUTPUTS "sentiments.xlsx"

- Use the first code chunk once for each new sheet to create the sheet, however, do change the way of saving to excel file accordingly as one creates the excel file, and one amend the excel file to contain the new sheet
    - IMPT because the first way rewrites the entire file so would have to restart
- Use the second code chunk to amend to the dataframe to include the new columns, i.e., the returned results from ChatGPT