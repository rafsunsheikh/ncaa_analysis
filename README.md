## **Section 1: Data Preprocessing**
### **Followed steps for 1_data_preprocessing/1_data_loading:**
1. Data loading using Pandas 
2. Newspaper data parsing and extract of informations + Combine the data together
3. Reorder the columns of the data
#### **Initial data columns:** 
```
column_order = [
        'source_file', 'title', 'author', 'publication_info', 'abstract', 
        'urls', 'links', 'publication_title', 'publication_date', 'publication_year',
        'section', 'pages', 'publisher', 'place_of_publication', 'issn',
        'document_type', 'proquest_id', 'database', 'full_text'
    ]
```
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


### **Followed steps for 1_data_preprocessing/2_data_cleaning:**
1. Duplicate analysis 
2. Identify Articles Edition Varients
3. Identify publication patterns
4. Yearly distribution
5. Find titles that are very similar (potential typos)
6. Find articles with similar text across different publications
7. Suggest deduplication strategies based on analysis (conservative, moderate, aggressive)
8. Remove duplicates 
```
Loading data for duplicate analysis...
Loaded 173999 articles
=== DUPLICATE ANALYSIS ===
Total articles: 173999

1. EXACT DUPLICATES:
   Exact duplicates: 11283
   Sample exact duplicates:
 year                                                                    title                       publication_name
20 17                                          IN BRIEF [Corrected 06/04/2017]         Chicago Tribune; Chicago, Ill.
200 5 GOPHERS UPDATE ; Gophers aiming for sweep at Ohio State: [METRO Edition]       Star Tribune; Minneapolis, Minn.
 2002                'A Season on the Brink' Creators Cry Foul: [HOME EDITION] Los Angeles Times; Los Angeles, Calif.

2. SAME TITLE + YEAR (different editions/publications):
   Articles with same title+year: 37035
   Sample same title+year:
 year                                                                    title                       publication_name
20 17                                          IN BRIEF [Corrected 06/04/2017]         Chicago Tribune; Chicago, Ill.
20 17                                          IN BRIEF [Corrected 06/04/2017]         Chicago Tribune; Chicago, Ill.
200 5 GOPHERS UPDATE ; Gophers aiming for sweep at Ohio State: [METRO Edition]       Star Tribune; Minneapolis, Minn.
200 5 GOPHERS UPDATE ; Gophers aiming for sweep at Ohio State: [METRO Edition]       Star Tribune; Minneapolis, Minn.
 2002                'A Season on the Brink' Creators Cry Foul: [HOME EDITION] Los Angeles Times; Los Angeles, Calif.
 2002                'A Season on the Brink' Creators Cry Foul: [HOME EDITION] Los Angeles Times; Los Angeles, Calif.

3. EDITION VARIANTS:
   Edition variants: 627
   Sample edition variants:
year                                                               title                      publication_name
2002   'Gut feeling' brought Webb back to train at home: [FINAL Edition]                USA TODAY; McLean, Va.
2002 'Gut feeling' brought Webb back to train at home: [FIRST Edition 1]                USA TODAY; McLean, Va.
2002          After a Long Climb, Braswell Is Peaking: [FINAL Edition 1] The Washington Post; Washington, D.C.
2002            After a Long Climb, Braswell Is Peaking: [FINAL Edition] The Washington Post; Washington, D.C.

6. PUBLICATION PATTERNS:
   Total unique publications: 256
   Top 5 publications by article count:
     Chicago Tribune; Chicago, Ill.: 19852 articles
     The Washington Post; Washington, D.C.: 19487 articles
     Los Angeles Times; Los Angeles, Calif.: 17182 articles
     Boston Globe; Boston, Mass.: 16614 articles
     USA TODAY; McLean, Va.: 13455 articles

7. YEARLY DISTRIBUTION:
   Articles by year:
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

=== DEDUPLICATION RECOMMENDATIONS ===
Total potential duplicates: 48945
Potential final dataset size: 125054

RECOMMENDED DEDUPLICATION STEPS:
1. Remove exact duplicates (safe to remove)
2. For same title+year: keep one per publication
3. For edition variants: keep the most complete version
4. For wire articles: decide if you want multiple publications or just one

=== SAMPLE DEDUPLICATION RESULTS ===

Applying 'conservative' deduplication strategy...
Removed 11283 duplicates (6.5%)
Final dataset: 162716 articles

Applying 'moderate' deduplication strategy...
Removed 23927 duplicates (13.8%)
Final dataset: 150072 articles

Applying 'aggressive' deduplication strategy...
Removed 24057 duplicates (13.8%)
Final dataset: 149942 articles
149942
```
9. Quick CLeaning Verification:
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

### **Followed steps for 1_data_preprocessing/3_text_normalizer:**
1. Analyze current state of data to identify normalization needs
2. Text Normalization

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
3. Fix years with spaces or noise like '20 21' -> '2021'
4. Develop normalization pipeline (Levels: 'basic', 'moderate', 'advanced') and normalize


### **Followed steps for 1_data_preprocessing/3.5_normalized_data_cleaner:**
1. Remove specific Indices:
```
indices_to_delete = [144042,
5019,
175,
55680,
204,
149881,
37327,
94889,
145105,
104881,
138598,
37327,
149227,
80842,
31007,
828770,
115211,
126807,
89424,
149772,
17488,
144913,
37305,
126276,
93244,
79867,
1928,
143323,
7313,
149628,
144913,
71731,
108770,
64135,
59006,
138558]
```
2. Normalized data columns
```
columns=["year", "date", "title", "main_text", "publication_name", "title_clean", "publication_normalized", "year_normalized", "title_normalized", "main_text_normalized"]
```
3. Delete rows with specific words in the data:
```
words_to_delete = ["TV HIGHLIGHTS", "TV listing", "TV CRITIC’S Picks", "World-Wide"]
```
4. Remove all numbers from the "title_normalized" and "main_text_normalized" columns.
5. Remove words from the data
```
words_to_remove = ['v', 'pm', 'game', 'et', 'ppg', 'g', 'rpg', 'f', 'jr', 'apg', 'fg', 'sr', 'u']
```
6. Remove college names from the data:
```
words_to_remove_colleges = [
    'ucla', 'michigan', 'texas', 'carolina', 'florida', 'arizona', 'usc', 'john',
    'virginia', 'gopher', 'washington', 'duke', 'ohio', 'maryland', 'kansas',
    'kentucky', 'illinois', 'minnestoa', 'ou', 'oklahoma', 'iowa', 'california',
    'oregon', 'dame', 'notre', 'tennessee', 'georgia', 'alabama', 'indianna',
    'villanova', 'conneticut', 'stanford', 'chicago', 'wisconsin', 'angeles',
    'miami', 'asu', 'syracuse', 'louisville', 'huskies', 'utah', 'colorado',
    'uconn', 'houston', 'georgetown', 'lsu', 'unlv', 'baylor', 'missouri',
    'hawai', 'pittsburgh', 'philadelphia', 'las', 'auburn', 'nebraska', 'jersey',
    'harvard', 'arkansas', 'phoenix', 'cincinnati', 'indianapolis', 'atlanta',
    'clemson', 'marquette', 'louisianna', 'tampa', 'cleveland', 'umass',
    'princeton', 'austin', 'seattle', 'massechusettes', 'vanderbilt', 'orleans',
    'fransisco', 'minneapolis', 'milwaukee', 'northeastern', 'pepperdine',
    'delaware', 'pennsylvania', 'buffalo', 'yale'
]
```

### **Followed steps for 1_data_preprocessing/4_tokenization_and_stemming:**
1. Initialize Tokenizer (RegexpTokenizer from nltk.tokenize)
2. Remove common stopwords from nltk.corpus
3. Calculate frequency distribution
4. Initialize lemmatizer (WordNetLemmatizer from nltk.stem)
5. Merged data columns after tokenization
```
['date', 'publication_normalized', 'title_normalized', 'main_text_normalized', 'Title_tokenized', 'Main_text_tokenized']
```
6. Remove identified tokens
```
    tokens_to_remove = ['2024', 'ppg', 'g', 'rpg', 'f', 'jr', 'apg', 'fg', 'sr', 'ft', 'ppg', 'g', 'per', 'rpg', 'f', 'stats', 'jr', 'apg', 'fg']
```


### **Followed steps for 1_data_preprocessing/bigram tokenizer dictionary and corpus:**
1. Identify potential word segment for bigrams
```
selected_bigrams = [
    "basketball players",
    "field hockey",
    "story lines",
    "red storm",
    "season final",
    "st mary",
    "team final",
    "ten tournament",
    "tournament midwest",
    "ncaa lacrosse",
    "picks odds",
    "tournament games",
    "mock draft",
    "sports college",
    "tournament south",
    "basketball coaches",
    "basketball season",
    "college coaches",
    "conference tournament",
    "prediction picks",
    "football fyi",
    "mountain west",
    "ncaa regional",
    "soccer team",
    "va tech",
    "image likeness",
    "minnesota scene",
    "ncaa hopes",
    "nittany lions",
    "draft pick",
    "grand canyon",
    "jim harbaugh",
    "st thomas",
    "basketball player",
    "round final",
    "st louis",
    "west chester",
    "hockey team",
    "slow start",
    "tournament bracketology",
    "st round",
    "white house",
    "air force",
    "supreme court",
    "lacrosse final",
    "pac title",
    "win ncaa",
    "basketball games",
    "final final",
    "ncaa volleyball",
    "ncaa tournament",
    "college basketball",
    "college football",
    "march madness",
    "basketball tournament",
    "sports final",
    "ncaa women",
    "sun devils",
    "basketball coach",
    "ncaa basketball",
    "tar heels",
    "national title",
    "ncaa title",
    "nba draft",
    "ncaa tourney",
    "hall fame",
    "world series",
    "college sports",
    "stony brook",
    "caitlin clark",
    "blue devils",
    "top seed",
    "college athletes",
    "tournament final",
    "san diego",
    "football coach",
    "basketball team",
    "track morning",
    "football notebook",
    "division ii",
    "west regional",
    "national championship",
    "college world",
    "ncaa division",
    "track field",
    "ncaa final",
    "top seeded",
    "st joseph",
    "college baseball",
    "boston college",
    "chris dufresne",
    "nfl draft",
    "regular season",
    "ncaa bid",
    "local colleges",
    "division iii",
    "title final",
    "title ix",
    "college hockey",
    "east regional",
    "basketball ncaa",
    "super bowl",
    "water polo",
    "ncaa championship",
    "football playoff",
    "regional final",
    "head coach",
    "city edition",
    "top ranked",
    "gophers women",
    "north final",
    "win streak",
    "george mason",
    "ncaa berth",
    "penn st",
    "transfer portal",
    "basketball notebook",
    "winners losers",
    "college hoops",
    "midwest regional",
    "world cup",
    "wake forest",
    "campus angle",
    "rose bowl",
    "south regional",
    "falls short",
    "north dakota",
    "ivy league",
    "college roundup",
    "selection sunday",
    "athletic director",
    "dufresne college",
    "ole miss",
    "super regional",
    "tournament bracket",
    "tournament west",
    "lady vols",
    "basketball preview",
    "football team",
    "basketball report",
    "basketball roundup",
    "predictions picks",
    "uc irvine",
    "football players",
    "predictions odds",
    "hockey east",
    "sports ncaa",
    "bulldog edition",
    "ncaa baseball",
    "winning streak",
    "basketball insider",
    "college notebook",
    "tournament east",
    "ncaa softball",
    "games final",
    "holy cross",
    "student athletes",
    "wnba draft",
    "coaches poll",
    "sports betting",
    "win final",
    "coach final",
    "cross country",
    "madness bracket",
    "pac tournament",
    "rainbow wahine",
    "top seeds",
    "signing day",
    "title home",
    "basketball star",
    "basketball final",
    "basketball notes",
    "dawn staley",
    "football season",
    "j.a adande",
    "top pick",
    "east tournament",
    "ncaa championships",
    "top teams",
    "assistant coach",
    "college athletics",
    "ten title",
    "fall short",
    "ncaa president",
    "team usa",
    "bc women",
    "football notes",
    "gophers coach",
    "football preview",
    "robyn norwood",
    "football program",
    "football spotlight",
    "football coaches",
    "lady rebels",
    "ncaa field",
    "ncaa football",
    "title run",
    "madness games",
    "ncaa rules",
    "pga tour",
    "east region",
    "san jose",
    "deion sanders",
    "midwest region",
    "ncaas final",
    "red sox",
    "baseball team",
    "knee injury",
    "norwood college",
    "usf women",
    "free throws",
    "graduation rates",
    "rick pitino",
    "sun belt",
    "top coaches",
    "baseball coach",
    "basketball bruins",
    "final home",
    "hall famer",
    "acc tournament",
    "bill plaschke",
    "diane pucin",
    "eric sondheimer",
    "san antonio",
    "slam dunk",
    "straight win",
    "tournament schedule",
    "tournament title",
    "west coast",
    "basketball teams",
    "bowl games",
    "college players",
    "nick saban",
    "offensive line",
    "south region",
    "top college",
    "volleyball team",
    "prime time",
    "antelope valley",
    "williams arena",
    "center stage",
    "div ii",
    "east title",
    "ga tech",
    "ncaa soccer",
    "hoops coach",
    "losing streak",
    "t.j simers",
    "school football",
    "blue demons",
    "coach mike",
    "home final",
    "cal poly",
    "college lacrosse",
    "conference tournaments",
    "draft picks",
    "free throw",
    "lady toppers",
    "leading scorer",
    "ncaa sanctions",
    "c.w post",
    "mike krzyzewski",
    "ncaa tournaments",
    "white sox",
    "johns hopkins",
    "major college",
    "ncaa champion",
    "ncaa preview",
    "round pick",
    "senior guard",
    "fiesta bowl",
    "football player",
    "mark emmert",
    "ncaa track",
    "roy williams",
    "free agent",
    "juju watkins",
    "media day",
    "saint joseph",
    "america east",
    "angel reese",
    "defending champion",
    "ncaa record",
    "ncaa wrestling",
    "olympic trials",
    "rhode island",
    "softball tournament",
    "st bonaventure",
    "twin cities",
    "conference title",
    "girls basketball",
    "lacrosse tournament",
    "teams ncaa",
    "time ncaa",
    "knee surgery",
    "lacrosse team",
    "mike penner",
    "national team",
    "ncaa bracket",
    "sports gambling",
    "star power",
    "top recruit",
    "top spot",
    "coaching staff",
    "college softball",
    "crimson tide",
    "gophers basketball",
    "gophers volleyball",
    "hockey tournament",
    "ncaa meet",
    "record final",
    "soccer coach",
    "top player",
    "top scorer",
    "coast conference",
    "female athletes",
    "junior college",
    "lebron james",
    "college career",
    "college volleyball",
    "free agency",
    "football college",
    "paige bueckers",
    "selection committee",
    "st cloud",
    "wins ncaa",
    "recruiting class",
    "home run",
    "james madison",
    "southern cal",
    "alma mater",
    "arena sports",
    "basketball program",
    "heisman trophy",
    "minnesota duluth",
    "ncaa hockey"
]
```

2. Replace the `" "` between words with `"_"` to make them bigrams
3. Create and store "Dictionary" and "Corpus" from the tokens in the data for LDA training and analysis

## **Section 2: Training**
### **Followed steps for 2_training_scripts/NCAA training Models (Applies for 10, 15, 20, 25, 30, 35, 40, 45 and 50 topics models) :**
1. Load the dictionary and corpus
2. Start the LDA (Latent Dirichlet Allocation) Training
```
lda_model_15_topics = LDA(corpus=corpus, id2word=dictionary, num_topics=15, random_state=100,
                chunksize=1000, passes=25,iterations=50)
## corpus: Tokens are stored in Gensim’s bag-of-words format (list of (token_id, count) pairs per doc).

## id2word=dictionary: mapping from token IDs to actual tokens (gensim.corpora.Dictionary). ## Lets LDA translate IDs ↔ words.

## num_topics=15: asks the model to discover 15 topics.

## random_state=100: fixes the RNG seed → reproducible results.

## chunksize=1000: number of documents processed per training chunk (memory/perf trade-off).

## passes=25: how many times the algorithm will pass over the entire corpus (more passes → more thorough, slower).

## iterations=50: number of internal Gibbs/variational updates per document within each pass (more iterations → better convergence, slower).

```
3. Save the model in `.gensim` format
4. Find the coherence value `c_v` of the topics with the documents (model used `CoherenceModel` from `gensim.models`) 


## **Section 3: Topic Analysis**
### **Followed steps for 3_multiple_topics_loader_and_analyzer/NCAA LDA Analysis  <topic_numner> Topics Loading and Analysis (Applies for 10, 15, 20, 25, 30, 35, 40, 45 and 50 topics models) :**
1. Load the trained model
2. Load the dictionary and corpus
3. Load all documents
4. Get the `Dominant topic`, `Percentage Contribution`  and `Keywords` for each document
```
Document_No	Dominant_Topic	Topic_Perc_Contrib	Keywords	Text
0	0	4	0.2835	team, players, lot, time, play, playing, coach...	[coaching, living, bonnie, henrickson, phone, ...
1	1	21	0.2231	family, school, life, home, father, basketball...	[sidebar, ruston, la, dining, louisiana, cajun...
2	2	43	0.2648	fans, time, people, sports, moment, day, fan, ...	[garden, missed, lonely, nights, season, debut...
```
5. Sort documents within each topic by percentage contribution in descending order
6. Get the total number of documents for each dominant topic
7. Get the top 100 documents for each topic
8. Get all the documents for each topic
9. Save the dataframe as excel

## **Section 4: Data Postprocessing**
### **Followed steps for 4_data_postprocessing/Top 100 documents add +2 with the document number:**
1. Load the document list excel file
2. Add +2 with the index numbers for each document

