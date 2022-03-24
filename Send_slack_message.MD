# Programmatically Send Slack Messages Using Python.

### Step 1:
* Create a app inside Slack and create a app level token with `channel write access`
  * In your Python script assign app level token  to `slack_app_token` variable
* Retrieve the bot user OAuth token from the OAuth and permissions page
  * In your Python script assign the bot user OAuth token  to `slack_token` variable

### Step 2:
Install the following modules
```
pip install requests
pip install time
```

### Step 3:
* Inside Slack create a channel called `#python_api`

### Step 4:
Import necessary modules 
```
  import requests
  import time
  ```
 
Retrieve epoch time
```
now_in_unix=int(time.time())
```
Assign credentials

```
slack_app_token ='xapp-1-REDACTED'
slack_channel = '#python_api'
slack_token = 'xoxb-REDACTED'
```
Function to post the message
  ```
  def post_message_to_slack(text, blocks = None):
    
    requests.post('https://slack.com/api/chat.postMessage', {
        'token': slack_token,
        'channel': slack_channel,
        'text': text
    })
    return'Success' 
```
Calling the function
```
post_message_to_slack(text=f'Time elapsed in Epoch is  {now_in_unix}')
```

