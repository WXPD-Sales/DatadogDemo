<p align="center">
  <h2 align="center"> Datadog Demo</h2>

  <p align="center">
    This bot demo showcases the art of the possible, when it comes to how notifications from Datadog could be implemented into Webex messaging, using Adaptive Cards and user collaboration.
    <br />
    <a href="https://youtu.be/s6VqLXaaACM"><strong>View Demo</strong></a>
    ·
    <a href="https://github.com/WXSD-Sales/DatadogDemo/issues"><strong>Report Bug</strong></a>
    ·
    <a href="https://github.com/WXSD-Sales/DatadogDemo/issues"><strong>Request Feature</strong></a>
  </p>
</p>

## About The Project

### Video Demo

[![Datadog Video Demo](https://img.youtube.com/vi/s6VqLXaaACM/0.jpg)](https://youtu.be/s6VqLXaaACM, "Datadog Video Demo")


### Built With

- Python3 (v3.7.4)
- Tornado v4.5.2 (Python Web Framework)

<!-- GETTING STARTED -->


### Installation & Usage

1. Clone the repo
   ```sh
   git clone repo
   ```
2. ```
   pip install tornado==4.5.2
   ```
3. The following Environment variables are required for this demo, set with the appropriate values:
   ```
   MY_BOT_ID=1234567890
   MY_BOT_TOKEN=ABCDEFG-1234-567A_ZYX
   MY_SECRET_PHRASE=datadogtestexample
   MY_BOT_PORT=8080
   DD_API_KEY=abcdefghijklmnop
   DD_APPLICATION_KEY=31415826753492031
   DD_CREATOR_ID=abcdefgh-12ab-34cd-56ef-ijklmnopqrstu
   ```
4. To run this, you can simply run:
   ```
   python datadog_reply.py
   ```
5. Using the bot's token, you can get the bot's personId here:
   https://developer.webex.com/docs/api/v1/people/get-my-own-details

   The DD_API_KEY and DD_APPLICATION_KEY Values need to be setup through Datadog:
   https://docs.datadoghq.com/api/latest/authentication/

   You can use the Datadog API (https://docs.datadoghq.com/api/latest/users/) to find an appropriate DD_CREATOR_ID value (this will be the user who creates the incident in Datadog).
6. You will need to create at least one Webex [webhook](https://developer.webex.com/docs/api/guides/webhooks) to your bot's hosted location + /cards, example:
https://yoursite.com/cards - required fields:
   ```
      "name": "Attachment Actions Webhook",
      "targetUrl": "https://yoursite.com/cards",
      "resource": "attachmentActions",
      "event": "created",
   ```
7. In addition, you will need a webhook on the Datadog side (https://app.datadoghq.com/account/settings#integrations/webhooks).  The webhook payload you setup should look like this:
   ```
   URL: https://webexapis.com/v1/messages
   ```
   You will need to replace the roomId with a valid Webex [Room (space)](https://developer.webex.com/docs/api/v1/rooms)
   ```
   {
     "roomId": "XXXX",
     "markdown": "DataDog Example Alert",
     "attachments": [
       {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": 
      {
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "TextBlock",
            "size": "Medium",
            "weight": "Bolder",
            "text": "Alert!"
        },
        {
            "type": "FactSet",
            "facts": [
                {
                    "title": "ID",
                    "value": "$ID"
                }
            ]
        },
        {
            "type": "FactSet",
            "facts": [
                {
                    "title": "HOSTNAME",
                    "value": "$HOSTNAME"
                }
            ]
        },
        {
            "type": "TextBlock",
            "text": "$EVENT_MSG",
            "wrap": true
        },
        {
            "type": "ActionSet",
            "actions": [
                {
                    "type": "Action.Submit",
                    "title": "Acknowledge",
                    "data":{"id":"$ID", "submit":"ack"}
                },
                {
                    "type": "Action.OpenUrl",
                    "title": "View",
                    "url":"$LINK"
                },
                {
                    "type": "Action.ShowCard",
                    "title": "New Incident",
                    "card":{"type": "AdaptiveCard",
                            "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
                            "version": "1.2",
                            "body": [
                                    {
                                        "id":"title",
                                        "type": "Input.Text",
                                        "placeholder": "Incident Title",
                                        "spacing": "default"
                                    },
                                    {
                                        "type": "ActionSet",
                                        "actions": [
                                            {
                                                "type": "Action.Submit",
                                                "title": "Create Incident",
                                                "data": {
                                                    "hostname": "$HOSTNAME",
                                                    "url": "$LINK",
                                                    "submit": "inc"
                                                }
                                            }
                                        ]
                                    }
                                ]
                        }
                }
            ]
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.2"
      }
   }
   ]
   }
   ```
   You will also need to specify Custom Headers, where the Bearer Token value is replaced by the one you get from [creating a bot of your own.](https://developer.webex.com/my-apps)
   ```
   {"Content-Type":"application/json", "Authorization":"Bearer XXXX_PF84_1eb65fdf-9643-417f-9974-ad72cae0e10f"}
   ```

## License

Distributed under the MIT License. See `LICENSE` for more information.

<!-- CONTACT -->

## Contact
Please contact us at wxsd@external.cisco.com
