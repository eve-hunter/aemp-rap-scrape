# Oakland Rent Adjustment Program (RAP) scrape
This is a scrape of https://apps.oaklandca.gov/rappetitions/SearchCases.aspx

.
├── cleaning
│   └── rap-scrape-cleaning-pipeline.ipynb        
├── codebooks           
├── data      
│   ├── clean
│   │	├── case_progress.csv
│   │	├── rap_cases_clean.csv
│   │	└── top_level_data.csv
│   ├── raw
│   └── top-level     
├── scraper
│   └── rap_scrape.py    
├── poetry.lock           
├── pyproject.toml
└── README.md


### ./scraper/rap_scrape.py
Contains the selenium-based scraper. You can define a date range, then run the scraper for that date range. It's reasonably well-documented in the .py file.

### ./data/raw/raw
I had the perform the initial scrape in chunks because I ran it in the background on my work machine and a crash would have been kinda devastating.

### ./data/raw/top-level
Contains the xlsx file that can easily be exported from the RAP website. Seems to be intended to include one entry for each unique combination of `petition_number`, `case_number`, and `grounds`/`reason`. However, a lot of data seems to be missing, and the file also doesn't contain addresses or apns.

### ./codebooks
Contains .csv files that map the unique substrings found in the `grounds` sections of the raw scrape to usable definitions.

### ./cleaning/rap-scrape-cleaning-pipeline.ipynb 
Contains the cleaning steps. Reads in & concatenates the raw scrape data, then parses the gnarly raw strings into cleaner strings. Also breaks out case progress data into a separate dataframe.

### ./data/clean/rap_cases_clean.csv
Contains the cleaned data from the scrape, minus case history, which is broken out into `case_progress.csv`
* `apn` is available for about 1/10 entries. `address` is available for about 99% of entries.
* `record_kind` differentiates who filed with the RAP.
* `Grounds`/`Reason` are split into a series of boolean columns. landlord reasons are preceded by `ll`, while tenant grounds are preceded by `ts`. These are extracted from the raw `grounds` columns using definitions in the codebooks.

### ./data/clean/rap_cases_cleancase_progress.csv
Each entry is an update in a case.