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

## 4. sentiments.ipynb

OUTPUTS "sentiments_cleaned.xlsx"

- Section "Preparation of Excel File"
- This can get confusing so, under "sentiments.xlsx", each sheet would have the headers - custom_id, categorical_x, categorical_y, numerical_x, numerical_y, numerical_z
    - I changed the header name manually on my own, but it would have been user_content if I remember correctly
- What I did was to also read the JSONL file used during ChatGPT API, one of them would be sufficient i.e. 2020_2024(1) or 2020_2024(5) is okay as what I needed was the custom_id and user_content (which contains the texts used)
- I then merged the two dataframe on "custom_id" under the "merged_df"
- Afterwards, I combined the "merged_df" with the imported dataframe of "texts_cleaned.xlsx" because I wanted the dates of the articles that I extracted previously
_ Similarly, do take note on the code lines for saving the file into Excel file

OUTPUTS "market_sentiments.xlsx"

- Section "Data Analysis"
- Firstly, map the categorical values, i.e., Positive, Negative, Neutral to 1, -1, 0 respectively
- Secondly, calculate the numerical difference between the numerical values

OUTPUTS "processed_market_sentiments.xlsx"

- Section "Market Sentiments"
- Actually, my robustness criteria was different from what was shared... 
    - Using the cumulative percentage for numerical difference..
        - Those below 60th percentile, which in general has a numerical difference of less than 0.2, I averaged out the numerical values
        - Those above 60th and below 85th percentile, which in general has a numerical difference of 0.2 to 0.4, I averaged out the two nearest numerical values
        - Those above 85th percentile, which in gneral has a numerical difference of more than 0.4, I averaged out all the five values

OUTPUTS "final_market_sentiments.xlsx"

- Section "Final Market Sentiments"
- Well, given the calculated sentiments for each text, I average out the value by grouping into Month, Quarter and Year respectively
