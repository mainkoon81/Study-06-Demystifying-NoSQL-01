# Data Wrangling with MongoDB

### 1. format
 - A common way to distribute **tabular data**(table) is in a format called 'CSV'.
<img src="https://user-images.githubusercontent.com/31917400/35482887-3d8fdaf0-0433-11e8-991c-c56e6af5647c.jpg" width="570" height="150" />
 
 - Parsing *huge..CSV file* in python (csv to dict)
   - Q.Let's creat 'one dictionary' for each item(row) in the file. Parse only first ten lines in the file.

Preparing data importing for later and preparing debugging my function. 
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

Write my function - step 2.
 - Here, row[0] is not the first list, but the first element of each list ! And these would become each value of the dictionary later on. Then we need to match those values with their keys (headers).
 - This means we need to control the iteration of entire single cycles. 
 - Ultimately, we want a list whose each element is a 'dictionary'. 
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

Test of my implemetation
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






































