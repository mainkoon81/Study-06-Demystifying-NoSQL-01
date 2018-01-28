# Data Wrangling with MongoDB

### 1. format
 - A common way to distribute **tabular data**(table) is in a format called 'CSV'.
<img src="https://user-images.githubusercontent.com/31917400/35482887-3d8fdaf0-0433-11e8-991c-c56e6af5647c.jpg" width="570" height="150" />
 
 - Parsing *huge..CSV file* in python (csv to dict)
   - Q.Let's creat 'one dictionary' for each item(row) in the file. Parse only first ten lines in the file.

Preparing data importing for later and preparing debugging of my function 
```
import os
DATADIR = "C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L5_DATABASE/data"
DATAFILE = "beatles-diskography.csv"

datafile = os.path.join(DATADIR, DATAFILE)
parse_file(datafile)
```
Write my function - step 1. 
 - FYI, this is the header 
<img src="https://user-images.githubusercontent.com/31917400/35484456-e869ba22-0447-11e8-8dbb-a47d159663a2.jpg" width="650" height="25" /> 

```
def parse_file(datafile):
    data = []
    with open(datafile, "r") as f:
        
        # Get header names        
        header = f.readline().split(',')
        # reading headers of 'f' can only be done using 'readline()' ? 
        
        # Loop through all lines in the file 
        for i, line in enumerate(f, start = 0):
            # only take first ten rows (ie. i == 0 to 9)
            if i == 10:
                break
            row = line.split(',') # parse individual line
            print(row)   # we can see 'row' is an bunch of lists !
```
<img src="https://user-images.githubusercontent.com/31917400/35484401-428f74d4-0447-11e8-8f71-c72e50cdea4d.jpg" width="570" height="100" />

Write my function - step 2.
 - Here, row[0] is not the first list, but the first element of each list ! 
 - 








































