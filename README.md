# Data Wrangling with MongoDB
#### Datasets
 - ERCOT data files: http://www.ercot.com/gridinfo/load/load_hist/
 - Musicbrainz JSON files: https://musicbrainz.org/doc/Development/JSON_Web_Service
 - TranStats Flight Data site: https://www.transtats.bts.gov/Data_Elements.aspx?Data=2
 - OpenStreetMap data: http://www.openstreetmap.org/#map=6/53.464/-8.218, https://mapzen.com/data/metro-extracts/metro/chicago_illinois/85940195/Chicago/, https://mapzen.com/data/metro-extracts/ 
#### modules
 - pip install: requests xlutils beautifulsoup4 pymongo 

#### Contents
   - Study 1: Data Extraction Fundamentals
     - Assessing the Quality of Data
     - Intro to Tabular Formats
     - Parsing CSV
     - Parsing XLS with XLRD
     - Intro to JSON
     - Using Web APIs
   - Study 2: Data in More Complex Formats
     - Intro to XML
     - XML Design Principles
     - Parsing XML
     - Web Scraping
     - Parsing HTML
   - Study 3: Data Quality
     - What is Data Cleaning?
     - Sources of Dirty Data
     - Measuring Data Quality
     - A Blueprint for Cleaning
     - Auditing Validity
     - Auditing Accuracy
     - Auditing Completeness
     - Auditing Consistency
     - Auditing Uniformity
   - Study 4: Working with MongoDB
     - Data Modeling in MongoDB
     - Introduction to PyMongo
     - Field Queries
     - Projection Queries
     - Getting Data into MongoDB
     - Using mongoimport
     - Operators like $gt, $lt, $exists, $regex
     - Querying Arrays and using $in and $all Operators
     - Changing entries: $update, $set, $unset
   - Study 5: Analyzing Data
     - Examples of Aggregation Framework
     - The Aggregation Pipeline
     - Aggregation Operators: $match, $project, $unwind, $group
     - Multiple Stages Using a Given Operator
   - Study 6: Case Study - OpenStreetMap Data
     - Using iterative parsing for large data files
     - Open Street Map XML Overview
     - Exercises around OpenStreetMap data

### 1> Simple format: csv
 - package: `csv`
 - topic: A common way to distribute **tabular data**(table) is in a format called 'CSV'.
<img src="https://user-images.githubusercontent.com/31917400/35482887-3d8fdaf0-0433-11e8-991c-c56e6af5647c.jpg" width="570" height="150" />
 
 - Parsing *huge..CSV file* in python (from csv to dict)
   - Q.Let's creat 'one dictionary' for each item(row) in the file. Parse only first ten lines in the file.
## Ultimately, we want a **list** where each element is a 'dictionary'. 
>Preparing data importing for later and preparing debugging my function. 
```
import os
DATADIR = "C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L5_DATABASE/data"
DATAFILE = "beatles-diskography.csv"

datafile = os.path.join(DATADIR, DATAFILE)
parse_file(datafile)
```
>Write my function - step 1. 
 - FYI, this is the header 
<img src="https://user-images.githubusercontent.com/31917400/35484456-e869ba22-0447-11e8-8dbb-a47d159663a2.jpg" width="650" height="25" /> 

```
def parse_file(datafile):
    with open(datafile, "r") as f:
        
        ##### Get header names. Reading headers of 'f' can only be done using 'readline()' ? #####
        header = f.readline().split(',')
        
        ##### Loop through all lines in the file #####
        for i, line in enumerate(f, start = 0):
            ##### only take first ten rows (ie. i == 0 to 9) #####
            if i == 10:
                break
            row = line.split(',') ##### parse individual line #####
            print(row)   ##### we can see 'row' is a bunch of lists(individual lines) ! #####
```
<img src="https://user-images.githubusercontent.com/31917400/35484401-428f74d4-0447-11e8-8f71-c72e50cdea4d.jpg" width="570" height="100" />

>Write my function - step 2.
 - Here, row[0] is not the first list, but the first element of each list ! And these would become each value of the dictionary later on. Then we need to match those values with their keys (headers).
 - This means we need to control the iteration of entire single cycles. (n=0 ~ n+=1)
```
def parse_file(datafile):

    data = []
    
    with open(datafile, "r") as f:
        header = f.readline().split(',')
        
        n = 0
        
        for i in f:
            if n == 10:
                break
            row = i.split(',')  
            
            ##### initialise dictionary to store one line and match each colname and each value.. #####
            entry_dic = {}
            for j, value in enumerate(row, start = 0):
                entry_dic[header[j].strip()] = value.strip()  ##### matching keys and values, thus creating a dict # string method 'strip()' will come in handy to get rid of the extra whitespace (that includes newline character at the end of line)
            
            ##### append the dictionary to target list #####
            data.append(entry)
            
            n += 1
                
    return data[0], data[9] 
```
<img src="https://user-images.githubusercontent.com/31917400/35485944-97ed7554-045e-11e8-99e1-f42db0ccf3fb.jpg" width="650" height="180" />

>Test of my implemetation
```
def test():
    datafile = os.path.join(DATADIR, DATAFILE)
    d = parse_file(datafile)
    
    firstline = {'Title': 'Please Please Me', 'UK Chart Position': '1', 'Label': 'Parlophone(UK)', 'Released': '22 March 1963', 'US Chart Position': '-', 'RIAA Certification': 'Platinum', 'BPI Certification': 'Gold'}
    tenthline = {'Title': '', 'UK Chart Position': '1', 'Label': 'Parlophone(UK)', 'Released': '10 July 1964', 'US Chart Position': '-', 'RIAA Certification': '', 'BPI Certification': 'Gold'}

    assert (d[0]) == firstline
    assert (d[9]) == tenthline

    return True

test()
```

>QUESTION ? We know 'each field' is limited by ',' in a csv file. What if two values are enclosed in a quote ? 
## VARIATION: What if there are 'multiple values'(comma-contained) within a single field ?
 
That is where `'CSV' module` comes in. It deals with csv format in a complete way! --- `csv.DictReader()`
 - Read all our data into dictionaries ! (all individual element becomes a dict) 
 - it automatically assumes, the first row of whatever file we read, is actually a header row and those are names we want to use for fields.
 - it automatically handles quote char ('ad'), quoted field ('ad,bd'), and so on.
 - No need to control the iteration ? No need to care headers ?
```
import os
import pprint
import csv

DATADIR = "C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L5_DATABASE/data"
DATAFILE = "beatles-diskography.csv"

def parse_csv(datafile):
    
    data = []
    
    with open(datafile, 'r') as f:
        book = csv.DictReader(f)
        for i in book:
            data.append(i)
    return(data)
```
print the 10th row !
```
if __name__ == '__main__':
    datafile = os.path.join(DATADIR, DATAFILE)
    d = parse_csv(datafile)
    pprint.pprint(d[9])
```
<img src="https://user-images.githubusercontent.com/31917400/35486480-9d934562-0466-11e8-9b29-83977bc9b446.jpg" width="650" height="90" />

### 2> Simple format: XLS
 - package: `XLRD`
 - topic: 


































