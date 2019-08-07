# Setup Slack Connections on Airflow

[Home](../README.md)

If you want to use `SlackWebhookOperator` operator to post messages to Slack using incoming webhooks, which are recommended by Slack, you will need to setup a Slack connection. This is how we make it:

## Create a Slack app

![console](img/07_CreateApp.png?raw=true)

## Enable Incoming Webhooks

Choose `Incoming Webhooks`.

![console](img/07_IncomingWebhook.png?raw=true)

Then enable Webhook.

![console](img/07_EnableWebhook.png?raw=true)

## Create an Incoming Webhook

Click on `Add New Webhook to Workspace`.

![console](img/07_AddWebhook.png?raw=true)

You will see something similar to below image:

![console](img/07_AddWebhookResult.png?raw=true)

Pick a channel that the app will post to, and then click to `Install` your app. You’ll be sent back to your app settings, and you should now see a new entry under the Webhook URLs for Your Workspace section, with a Webhook URL:

![console](img/07_WebhookURL.png?raw=true)

That’ll look something like this:

```text
https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
```

Create an Airflow connection for Slack with HTTP connection and the part after `https://hooks.slack.com/services` should go under password:

```text
Host: https://hooks.slack.com/services
Conn Type: HTTP
Password: /T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
```

![console](img/07_AirflowConn.png?raw=true)

Create a Python function

```python
from airflow.hooks.base_hook import BaseHook
from airflow.contrib.operators.slack_webhook_operator import SlackWebhookOperator

SLACK_CONN_ID = 'slack'

def send_message(context):
    slack_webhook_token = BaseHook.get_connection(SLACK_CONN_ID).password
    slack_msg = "Your messages here"

    op = SlackWebhookOperator(
        task_id='slack_test',
        http_conn_id='slack',
        webhook_token=slack_webhook_token,
        message=slack_msg,
        username='airflow')
    return op.execute(context=context)
```
