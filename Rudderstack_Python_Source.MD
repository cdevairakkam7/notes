# Send Track Events via Python on RudderStack.

Step 1: Install RudderStack's official Python module. 

```
pip install rudder-sdk-python
```
Step 2: Import the rudder_analytics module 

```
import rudder_analytics
```
Step 3: Plugin RudderStack credentials 

```
rudder_analytics.write_key = "Write_key_inside_string"
rudder_analytics.data_plane_url = "Dataplane_url_inside_string"
```

Step 4: Sending "Song Played" track event via Python on RudderStack
```
rudder_analytics.track('User_id', 'Song Played', {
    'title': 'Despacito',
    'singer': 'Fonsi'
})
```
