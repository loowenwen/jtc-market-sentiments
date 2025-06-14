
# jtc-market-sentiments

**Note:** This project was developed during an internship with **JTC Corporation**. It explores the use of the **ChatGPT-4 API** for sentiment analysis of news articles related to Singapore's industrial property market. The final output is a structured dataset of **market sentiment indicators** across different time periods.

> ⚠️ Some file paths have been reorganized for better structure. Ensure your paths are updated if running any code directly — especially for the JSONL and Excel files.

---

## Project Structure

### `1. data_collection.ipynb`

**Outputs:** `urls.xlsx`, `texts.xlsx`

* Extracts URLs using Google Search (based on defined time period and keywords).
* Cleans URLs: removes duplicates and filters to five key news sources.
* Scrapes article dates using the URLs.
* Scrapes article content into `texts.xlsx`.

---

### `2. data_cleaning.ipynb`

**Outputs:** `texts_cleaned.xlsx`

* Initially saves partial cleaned texts to `data_cleaning.xlsx` when running cell-by-cell.
* `texts_cleaned.xlsx` contains a mix of cleaned/uncleaned articles until all sources are processed.
* Cleaning is done source by source (e.g., EdgeProp).
* For EdgeProp:

  * Many URLs failed text scraping.
  * Rescraping was attempted (twice); persistent failures were removed.
  * Manual copy-pasting was not pursued as article volume was sufficient.

---

### `3. chatgpt_batch.ipynb`

**Purpose:** Format and send JSONL files to ChatGPT Batch API
**Outputs:**

* JSONL files in `chatgpt/jsonl/`
* Results in JSONL files (e.g., `2010_2014.jsonl`)
* `sentiments.xlsx`

#### Key Notes:

* Generate JSONL files under "Generating JSONL File" section.
* Upload to: [OpenAI ChatGPT Batch API](https://platform.openai.com/batches)
* Batch token limit: 90,000 tokens.

  * Articles usually fine.
  * Long consultancy reports may need splitting.

#### Writing to Excel:

* The first code chunk **creates** an Excel file and should be run once.
* The second appends new sheets with ChatGPT results.

  * ⚠️ First method **overwrites** existing data — use cautiously.

---

### `4. sentiments.ipynb`

**Outputs:**

* `sentiments_cleaned.xlsx`
* `market_sentiments.xlsx`
* `processed_market_sentiments.xlsx`
* `final_market_sentiments.xlsx`

#### Breakdown:

**A. Cleaning Sentiments**

* "sentiments.xlsx" sheets renamed (e.g., from `user_content` to `categorical_x`, etc.).
* Merged `sentiments` results with:

  * JSONL input (`custom_id`, `user_content`)
  * Original texts (to retain article dates)

**B. Analysis**

* Mapped: Positive → 1, Neutral → 0, Negative → -1
* Computed numerical sentiment differences.

**C. Robustness Filtering**

* Applied percentile logic based on sentiment spread:

  * <60th percentile (∆ < 0.2): average all 5 values
  * 60–85th percentile (∆ 0.2–0.4): average 2 nearest
  * > 85th percentile (∆ > 0.4): average 5 values

**D. Final Output**

* Averaged sentiment scores by Month, Quarter, Year → `final_market_sentiments.xlsx`

---

### `5. market_sentiments.ipynb`

**Note:**

* Older file used for initial data exploration.
* Can be ignored.

---

### `6. rerun.ipynb`

**Note:**

* Used for re-running selected code segments from `sentiments.ipynb`.
* Can also be ignored.

---

### `7. consultancy_reports.ipynb`

**Purpose:**

* Applied same sentiment analysis logic to consultancy reports.

**Notes:**

* Texts were extracted via a different notebook (to be added to repo).
* Sentiment score for each report = **average of two nearest numbers** (due to variance across prompts).
* Modify `sheet_name`, `output_file` when generating JSONL for this part.

---

## 📂 `text extraction for consultancy reports` folder

### `1. text_extraction.ipynb`

* Extracts text from PDFs in bulk.

### `2. image_extraction.ipynb`

* Extracts text from image-based articles (e.g., EdgeProp screenshots).

### `3. text.ipynb`

* Extracts text from a **single PDF** (used for manual retries).

### `4. image.ipynb`

* Extracts text from a **single image** (used for manual retries).

---

## Summary

This project shows a full pipeline for:

1. Scraping and cleaning news data
2. Structuring content for ChatGPT-4 batch processing
3. Interpreting and refining model outputs
4. Producing a time-series of **industrial market sentiments**.

For detailed logic and decisions, refer back to the individual notebook sections.
