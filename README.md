Bird's REST API for Python
=================================
This repository contains the open source Python client for New MessageBird's REST API. Documentation can be found at: https://docs.bird.com/api/.


Requirements
------------
- [Sign up](https://www.messagebird.com/en/signup) for a free MessageBird account
- Create a new access key in the developers sections
- An application written in Python (tested with Python 2.7 and Python 3.4)

Installation
------------
The easiest way to install the bird_python package is either via pip:

```
$ pip install bird_python
```

Examples
--------
We have put some self-explanatory examples in the [examples](https://github.com/messagebird/python-rest-api/tree/master/examples) directory, but here is a quick example on how to get started. Assuming the installation was successful, you can import the messagebird package like this:

```python
import bird_python as messagebird
```

Then, create an instance of **messagebird.Client**:

```python
client = messagebird.Client('YOUR_ACCESS_KEY', 'YOUR_ORGANIZATION_ID')
```

Now you can query the API for information or send a request. For example, if we want to send a message through any channel like whatsapp, you'd do something like this:

```python
try:
  # Get and set any workspace for sent request to the channels related.
  client.workspaces = client.workspace_list()['results'][0]['id']
  # Get and set any channel id for sender request, for example: Whatsapp, Messenger, SMS
  channels_id = client.channel_list()['results'][0]['id']
  
  # Build the body message like this format
  body = {
        "receiver": {
            "contacts": [
                {
                    "identifierValue": "+15598585"
                }
            ]
        },
        "body": {
          "type": "text",
          "text": {
            "text": "Single text message"
          }
        }
    }
  
  response = client.send_message(channels_id=channels_id, body=body)

  # Print the object information.
  print('response:\n')
  print(response)

except messagebird.client.ErrorException as e:
  print('Error:\n')

  for error in e.errors:
    print('  code        : %d' % error.code)
    print('  description : %s' % error.description)
    print('  parameter   : %s\n' % error.parameter)

```


Actions
-------


1. **Get Contact**
```python
contact = client.contact(uuid='YOUR_CONTACT_ID')
# Retrieve the information of a specific contact.
```

2. **Create Contact**
```python
body = {
        "displayName": "Jhon Doe",
        "identifiers": [
                {
                    "key": "phone",
                    "value": "+15551234"
                }
            ]
        }
contact = client.contact_create(body=body)
# Allows create a contact.
```

3. **Get Contact List**
```python
contact = client.contact_list()
# Extract all contacts created in a workspace.
```

4. **Get Workspace List**
```python
contact = client.workspace_list()
# Gets a list of workspaces created in an organization's account.
```

5. **Get Workspace By ID**
```python
contact = client.workspace_list_by_id(uuid='YOUR_WORKSPACE_ID')
# Gets a list of workspaces created in an organization's account.
```

6. **Get Project List**
```python
contact = client.project_list()
# Gets a list of projects created in an workspace.
# In this API you get what were previously known as templates,
# only now they can be compatible with multiple channels.
```

7. **Get Project By ID**
```python
contact = client.project_list_by_id(uuid='YOUR_PROJECT_ID')
# Gets one project created in an workspace by id.
```

8. **Get Channel List**
```python
contact = client.channel_list()
# Gets a list of channels created in an workspace.
```

9. **Get Channel By ID**
```python
contact = client.channel_list_by_id(uuid='YOUR_CHANNEL_ID')
# Gets one channel created in an workspace by id.
```

10. **Get Connector List**
```python
contact = client.connector_list()
# Gets a list of connectors created in an workspace.
# A connector can be Whatsapp, Messenger, SMS
```

11. **Get Connector By ID**
```python
contact = client.connector_list_by_id(uuid='YOUR_CONNECTOR_ID')
# Gets one connector created in an workspace by id.
```

12. **Send Message**
```python
body = {
        "receiver": {
            "contacts": [
                {
                    "identifierValue": "+15551234"
                }
            ]
        },
        "body": {
          "type": "text",
          "text": {
            "text": "Single text message"
          }
        }
    }
contact = client.send_message(channels_id='YOUR_CHANNEL_ID', body=body)
# Allows you to send a message through a channel.
# :param channels_id: ID of the channel through which you want to send the message
# :param body: Body of the message in json, which contains all the information necessary to send the message
```

13. **Get Webhook Event List**
```python
contact = client.webhook_event_list()
# Gets a list of webhooks available in an workspace.
```

14. **Create Webhook**
```python
contact = client.webhook_create(body='body')
# Allows create a webhook based in a event of available webhook list.
```
