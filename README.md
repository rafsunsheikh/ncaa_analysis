#### **Followed steps for 1_data_preprocessing/1_data_loading:**
1. Data loading using Pandas 
2. Newspaper data parsing and extract of informations + Combine the data together
3. Reorder the columns of the data
4. Reorder and process all data based on years
5. Combined essential data (Sort by year)
    - Year of publication
    - Date of publication
    - Title
    - Main text (full_text)
    - Name of publication (publication_title)
6. Compact the data
7. Store data based on years in separate files
8. Exploratory Data Analysis (EDA)


#### **Followed steps for 1_data_preprocessing/1_data_loading:**
1. 






#### **Initial data columns:** 
```
column_order = [
        'source_file', 'title', 'author', 'publication_info', 'abstract', 
        'urls', 'links', 'publication_title', 'publication_date', 'publication_year',
        'section', 'pages', 'publisher', 'place_of_publication', 'issn',
        'document_type', 'proquest_id', 'database', 'full_text'
    ]
```


#### **Some initial Data Overview:**
```
==================================================
ESSENTIAL DATA SUMMARY
==================================================
Total articles: 173999
Years covered: 64 unique years
Year range: 20 02 to 2025
Unique publications: 257

Articles per year:
  20 02: 4 articles
  20 03: 1 articles
  20 04: 2 articles
  20 06: 4 articles
  20 07: 4 articles
  20 09: 1 articles
  20 10: 1 articles
  20 11: 1 articles
  20 12: 1 articles
  20 13: 1 articles
  20 16: 2 articles
  20 17: 3 articles
  20 18: 2 articles
  20 19: 5 articles
  20 21: 8 articles
  20 23: 4 articles
  20 24: 1 articles
  20 25: 2 articles
  200 2: 3 articles
  200 3: 6 articles
  200 4: 2 articles
  200 5: 3 articles
  200 6: 2 articles
  200 7: 2 articles
  2002: 10152 articles
  2003: 11333 articles
  2004: 10846 articles
  2005: 10569 articles
  2006: 9799 articles
  2007: 8673 articles
  2008: 12 articles
  2008.0: 8478 articles
  2009: 8022 articles
  201 0: 4 articles
  201 1: 2 articles
  201 2: 1 articles
  201 4: 4 articles
  201 5: 1 articles
  201 6: 1 articles
  201 7: 3 articles
  201 8: 2 articles
  201 9: 3 articles
  2010: 7973 articles
  2011: 7451 articles
  2012: 7715 articles
  2013: 6543 articles
  2014: 6028 articles
  2015: 6147 articles
  2016: 6027 articles
  2017: 5631 articles
  2018: 5679 articles
  2019: 6244 articles
  202 0: 4 articles
  202 1: 3 articles
  202 2: 4 articles
  202 3: 4 articles
  202 4: 4 articles
  202 5: 4 articles
  2020: 5038 articles
  2021: 5334 articles
  2022: 4636 articles
  2023: 5020 articles
  2024: 6387 articles
  2025: 4153 articles

Top 10 publications:
  Chicago Tribune; Chicago, Ill.: 19852 articles
  The Washington Post; Washington, D.C.: 19487 articles
  Los Angeles Times; Los Angeles, Calif.: 17182 articles
  Boston Globe; Boston, Mass.: 16614 articles
  USA TODAY; McLean, Va.: 13455 articles
  Philadelphia Inquirer; Philadelphia, Pa.: 13411 articles
  Star Tribune; Minneapolis, Minn.: 12638 articles
  Arizona Republic; Phoenix, Ariz.: 11132 articles
  Newsday, Combined editions; Long Island, N.Y.: 10742 articles
  USA Today (Online); Arlington: 10443 articles

Data quality:
  Articles with titles: 173999
  Articles with main text: 173999
  Articles with publication name: 173999

Output file: essential_newspaper_data.csv
File size: 750.77 MB

Sample data (first 3 rows):
 year                                                                                                                               title                              publication_name
20 02 A Sudden Tragedy, a Long Recovery; After Illness and Amputations, Virginia Tech's DuBose Discovers a World of Help: [FINAL Edition]         The Washington Post; Washington, D.C.
20 02                         Down-home coach leads La. Tech ; Barmore's program short on funds but not short on success: [FINAL Edition]                        USA TODAY; McLean, Va.
20 02                                                                  It's a Must for Latrell to Play Well - Next Season: [ALL EDITIONS] Newsday, Combined editions; Long Island, N.Y.
```

#### **Initial Data Cleaning checkpoint:**
```
✅ Cleaned dataset saved to: cleaned_newspaper_data.csv
📊 Dataset summary:
   - Original articles: 173,999
   - Duplicates removed: 24,057
   - Final articles: 149,942
   - Reduction: 13.8%
   - File size: 673.47 MB

📋 Quick verification:
   - Columns: ['year', 'date', 'title', 'main_text', 'publication_name', 'title_clean']
   - Year range: 20 02 to 2025
   - Unique publications: 248

📝 Sample of cleaned data:
 year                                                                                                                               title                              publication_name
20 02 A Sudden Tragedy, a Long Recovery; After Illness and Amputations, Virginia Tech's DuBose Discovers a World of Help: [FINAL Edition]         The Washington Post; Washington, D.C.
20 02                         Down-home coach leads La. Tech ; Barmore's program short on funds but not short on success: [FINAL Edition]                        USA TODAY; McLean, Va.
20 02                                                                  It's a Must for Latrell to Play Well - Next Season: [ALL EDITIONS] Newsday, Combined editions; Long Island, N.Y.
```

##### **Common Encoding Issue Fix:**
```
replacements = {
        "â€™": "'",
        "â€œ": '"',
        "â€\x9d": '"',
        "â€": '"',
        "â€“": "-",
        "â€”": "—",
        "â€\"": "—",
        "Â": "",
        "\x92": "'",
        "\x93": '"',
        "\x94": '"',
        "\x96": "-",
        "\x97": "—",
    }
```

##### **Applied common normalization rules:**
```    
    normalization_rules = {
        r";\s*.*$": "",        # remove trailing semicolon segments
        r",\s*.*$": "",        # remove trailing comma segments
        r"\s*\(.*\)": "",      # remove parentheses content
        r"^the\s+": "",        # drop leading "the "
        r"\s+": " ",           # collapse whitespace
    }
```

##### **Removed word contractions:**
```
contractions = {
        "won't": "will not",
        "can't": "cannot",
        "n't": " not",
        "'re": " are",
        "'ve": " have",
        "'ll": " will",
        "'d": " would",
        "'m": " am",
    }
```

##### **Initial Stopwords Removal:**
```
stop_words = {
            "the","a","an","and","or","but","in","on","at","to","for","of","with","by","from","up",
            "about","into","through","during","before","after","above","below","between","among",
            "this","that","these","those","i","me","my","myself","we","our","ours","ourselves",
            "you","your","yours","yourself","yourselves","he","him","his","himself","she","her",
            "hers","herself","it","its","itself","they","them","their","theirs","themselves",
            "what","which","who","whom","whose","am","is","are","was","were","be","been","being",
            "have","has","had","having","do","does","did","doing","will","would","should","could",
            "can","may","might","must","shall",
        }
```

