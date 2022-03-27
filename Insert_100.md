# Insert ~927k Rows Into Snowflake via Python

## CSV Structure  
* [CSV Link](https://github.com/cdevairakkam7/notes/blob/main/dataset/annual-enterprise-survey-2020-financial-year-provisional-csv.csv.zip)
* The CSV has ~927k rows and 10 columns. 
* The CSV contains 159.8MB.

## Plan 
* Insert this mega CSV without breaking Snowflake or Python limitations 

## Installation of Python Modules 
```
pip install csv 
pip install requests 
pip install sqlalchemy
pip install --upgrade snowflake-sqlalchemy
```

## Calling the Modules 
```
import csv
from snowflake.sqlalchemy import URL
from sqlalchemy import create_engine
```

## Openining the file 
```
/* Note : Unzip the csv before opening it */
file_object = open("https://github.com/cdevairakkam7/notes/blob/main/dataset/annual-enterprise-survey-2020-financial-year-provisional-csv.csv.zip")
csv_reader=csv.reader(file_object)
```

## Creation of Table 
```
engine = create_engine(URL(
            account = 'fva01610',
            user = 'cdaniel7',
            password = 'Amplitude20DEC!',
            database = 'hackerrank',
            schema = 'test_schema',
            warehouse = 'compute_wh',
            autocommit=True
        ))

connection = engine.connect()
sql_string =f'Create or replace table enterprise_survey(Year  string,Industry_aggregation_NZSIOC string, Industry_code_NZSIOC string, Industry_name_NZSIOC string, Units string, Variable_code string, Variable_name string, Variable_category string, "Value" string, Industry_code_ANZSIC06 string)'
connection.execute(sql_string)
sql_string=[]
out=0
```

## Insertion of Data 
```
for line in csv_reader:
    if line[1] !='Industry_aggregation_NZSIOC':
        sql_string.append (f"('{line[0]}','{line[1]}','{line[2]}','{line[3]}','{line[4]}','{line[5]}','{line[6]}','{line[7]}','{line[8]}','{line[9]}'),")
        if len(sql_string)%50 == 0:
            insert_string =f"insert into enterprise_survey values({sql_string})"
            insert_string=insert_string.replace('["(','')
            insert_string=insert_string.replace('", "','')
            insert_string=insert_string.replace('),"])',')')
            sql_string==[]
            connection.execute(insert_string)
            insert_string=[]
```
