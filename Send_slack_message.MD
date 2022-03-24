# Programatically Send Slack Messages Using Python.

### Step 1:
* Create a app inside slack and create a  app level token with channel write access
* Retrive the bot user OAuth token from the OAuth and permissions page

### Step 2:
Install the following module
```
pip install requests
pip install time
```

### Step 3:
* Inside Slack create a channel called `#python_api`

### Step 4:
* Run the following code 
```
# import necessary modules 
  import requests
  import time
 
 # Retrieve Epoch time
 now_in_unix=int(time.time())
  
  
  # Assign Credentials 
  slack_app_token ='xapp-1-REDACTED'
  slack_channel = '#python_api'
  slack_token = 'xoxb-REDACTED'





  # Function to post the message
  def post_message_to_slack(text, blocks = None):
    
    requests.post('https://slack.com/api/chat.postMessage', {
        'token': slack_token,
        'channel': slack_channel,
        'text': text
    })
    return'Success' 

    # Calling the function
    
   post_message_to_slack(text=f'Time elapsed in Epoch is  {now_in_unix}')

  return 'Success'
