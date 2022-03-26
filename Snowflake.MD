# Create, Insert, Update, Delete, etc. In Snowflake via Python

### Step-1:
Installation of Python modules
```
pip install sqlalchemy
pip install --upgrade snowflake-sqlalchemy
pip install requests 
```
### Step-2:
Importing required modules 
```
from snowflake.sqlalchemy import URL
from sqlalchemy import create_engine
```
### Step-3:
Establishing connection with Snowflake 
```
engine = create_engine(URL(
            account = 'REDACTED',
            user = 'REDACTED',
            password = 'REDACTED!',
            database = 'REDACTED',
            schema = 'REDACTED',
            warehouse = 'REDACTED',
            autocommit=True
        ))
connection = engine.connect()
```
### Create Table:
```
sql_string =f"create or replace table python_api_table(car string, model string, mileage int)"
connection.execute(sql_string)
```

### Insert Value:
```
sql_string =f"insert into  python_api_table values('Mustang', 'Ford', 100)"
connection.execute(sql_string)

```

### Update Value 
```
sql_string =f"update python_api_table set car ='Mustang Eco Boost' where mileage =100"
connection.execute(sql_string)
```

### Delete Value
```
sql_string =f"delete from python_api_table where mileage =100"
connection.execute(sql_string)
```

### Truncate Table
```
sql_string =f"truncate python_api_table"
connection.execute(sql_string)
```

### Drop Table
```
sql_string =f"drop table python_api_table"
connection.execute(sql_string)
```
