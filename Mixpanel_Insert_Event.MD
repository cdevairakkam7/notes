# Insert Events Data Into Mixpanel API via Python

### Task - Insert the following events via Python
* Video Played 
* Video Paused 
* Video Resumed 

### Step 1:
* The data that needs to be inserted should be in a specific pattern like [this](https://github.com/cdevairakkam7/notes/blob/main/mixpanel_event.txt).

### Step 2:
* Retrieve `API_secret` and `Token` from Mixpanel console [[ref-link](https://help.mixpanel.com/hc/en-us/articles/115004490503-Project-Settings)].   

### Step 3:
* Install required Python modules 
  * ```Python 
       pip install requests
       pip install mixpanel_utils 
       ```
### Step 4: 
Python Script
```
#Import the necessary module
from mixpanel_utils import MixpanelUtils

if __name__ == '__main__':
    
    #Plug in the credentials here
    credentials = {'API_secret': 'XXX','token': 'YYY',}
    
    import_event = MixpanelUtils(credentials['API_secret'],credentials['token'])
    import_event.import_events('https://github.com/cdevairakkam7/notes/blob/main/mixpanel_event.txt', timezone_offset=0)
```
### Step 5:
* Check if the data is being reflected on Mixpanel console in near real-time.
