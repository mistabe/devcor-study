# Cisco DEVCOR 350-901 Study Guide

<https://www.ciscopress.com/store/cisco-devnet-professional-devcor-350-901-study-guide-9780137500048>

## 3.0 Cisco Platforms

Interacting with Cisco products and their respective APIs is the body of this chapter.
The [Cisco DevNet Sandbox](https://developer.cisco.com/site/sandbox/) will assist here greatly so make sure you've an account which can access the sandbox for furnishing your journey.
You'll also need an account at the [Cisco Webex Developer](https://developer.webex.com/) portal

### 3.1 Construct API requests to implement ChatOps with Webex API

Certainly worth reading this before reading the study guide language on this objective.
<https://developer.webex.com/blog/from-zero-to-webex-chatbot-in-15-minutes-updated-for-2021>

Also worth getting used to the Webex Web UI. As a person that has never used Webex's current offering in an organisation, this means I have to get used to it's naming scheme and feature structure.

To allow your ChatOps code to work, you'll need to be either running your code on a publicly accessible service and URL, or you can spin up ngrok to facilitate testing from your local machine in a "firewall friendly" manner. Anyone that operates network security policy will likely _not_ call this "firewall friendly". You know who you are.

Sign up for an account at [Ngrok](https://ngrok.com/)

Download the ngrok executable to your Windows workstation

You'll need to authenticate your ngrok.exe to the service.
In the ngrok [Setup and Installation](https://dashboard.ngrok.com/get-started/setup) page you'll be shown how to authenticate for the first time. You'll be given a pre-formed command with your personal auth token similar to:

`./ngrok authtoken 2088LH...UEJTU`

The response will be stored to a configuration file: `C:\Users\<username>/.ngrok2/ngrok.yml`

Fire ngrok up from the location of the exe using `./ngrok http 7001 --region=eu` with "7001" being the port that the application will send data to your local app on for you and you'll see; 


```text
ngrok by @inconshreveable                                                                               (Ctrl+C to quit)                                                                                                                        Session Status                online                                                                                    Account                       Paul Beyer (Plan: Free)                                                                   Version                       2.3.40                                                                                    Region                        Europe (eu)                                                                               Web Interface                 http://127.0.0.1:4040                                                                     Forwarding                    http://<UID>.eu.ngrok.io -> http://localhost:7001                           Forwarding                    https://<UID>.eu.ngrok.io -> http://localhost:7001                                                                                                                                                  Connections                   ttl     opn     rt1     rt5     p50     p90                                                                             0       0       0.00    0.00    0.00    0.00      
```

INSERT NGROK DIAGRAM HERE

Webex Teams was renamed to Webex. Brilliant. The API service URLs we'll use will be the new URLs.

I'll correlate as much of this data as possible to using Slack APIs and reference them alongside the Webex APIs.

Chatbots help operations by simulating conversations. Chatbots can respond to question or even start automated workflows. Chatbots can be used as the follows; 

- Notifiers
- Controllers
- Personal Assistants

Have a read of this URL <https://developer.webex.com/docs/bots>

With Webex chatbots, the chatbot needs to listen to a webhook URL for Webex notifications. 
Obligatory alternate Slack documentation [here](https://api.slack.com/bot-users).
Practically speaking it's a small web server - read, "the Python Flask module" - that is serving HTTP requests and has to be publicly available using Ngrok for testing or hosted on a service of your choosing.

- Webex access token (gleaned from your developer portal)
- Webhook listener (Python Flask module)
- HTTP Library (Python requests module)
- Authetication Credentials (Meraki API key for example)

Your webhook [has to be registered](https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook) with the API Call

This will be the first request to "implement ChatOps with Webex API"

We'll register, show and delete webhooks.

I'm assuming you've got Python for Windows installed, created a Python virtual environment using Python for Windows and you've activated that virtual environemnt and used pip to install requests and flask
If not, read [here](https:\\insertguide.com) for a quick start. Perhaps you're even using [VS Code](https://code.visualstudio.com/) too, which would be lovely.

We'll build the code in parts.

```python
import requests

base_url = 'https://webexapis.com/v1'
#Use your actual bot access token below
bearer_token = 'ZDZkOT..bb8cd''
```

Create the variable "headers" and pass it a Python dictionary which will contain the headers for the payload the "authorization" key uses and [F-String](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals) to build the value.

```python
headers = {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {bearer_token}",
}
```

Create the variable "webhook" and pass it a Python dictionary with all the attributes required of it. Ensure your targetUrl value is your actual ngrok URL, using either the HTTPS or HTTP protocol both seem fine given both are created by ngrok and Webex doesn't care, also ensure the /webhooks path matches your flask configuration so you'll need `@app.route('/webhooks', methods=['POST'])` if you want the two to match, or you _could_, not saying you should remove the webhooks path both the WebEx webhook and Flask configuration and roll like that instead.

```python
webhook = {
    "name": "Bobs bitch tits",
    "targetUrl": "http://<yourngrokGUID>.ngrok.io/webhooks",
    "resource": "messages",
    "event": "created",
}
```

Read current webhooks and optionally display them

```python
response = requests.get(f"{base_url}/webhooks", headers=headers, json=webhook)
response.raise_for_status()
response.content()
```

Based on retrieving the webhooks to the variable "response", delete the webooks one by one

```python
response = requests.get(f"{base_url}/webhooks", headers=headers, json=webhook)
response.raise_for_status()

for item in response.json()["items"]:
    print (f'Deleting webhook \"{item["name"]}\"...')
    complete_delete = requests.delete(f'{base_url}/webhooks/{item["id"]}', headers=headers)
    complete_delete.raise_for_status()
    print (complete_delete.status_code)
```

Finally create a webhook. Note the use of the HTTP POST method to achieve it

```python
response = requests.post(f"{base_url}/webhooks", headers=headers, json=webhook)
response.raise_for_status()
```

I've oberserved the code makes use of the requests module's "raise_for_status" quite often.
Useful to know that the `.raise_for_status()` method only plays a part when the response is an error of some kind not in the 2xx HTTP response code range.

You could use `.status_code()`, which is kind and gives you the code regardless of which code number it is in the successes or failures. Then should you be working programtically with that response gives you more work to do if the response is a successful one.

I choose to remember Try/Except blocks as "teef" like an illiterate way of saying "teeth" because there's always the optional else and finally clauses available to you.
Exceptions can - and should - be raised in a more specific to less specific descending order.

```python
import requests 

url = "https://www.cisco.com"
try:
    response = requests.get(url)
    # If the response was successful, no Exception will be raised
    response.raise_for_status()
except requests.exceptions.HTTPError as http_err:
    print(f'HTTP error occurred: {http_err}')
except Exception as err:
    print(f'Other error occurred: {err}')
```

Webhooks that are created have to be registered, documnetation on how to do that [here](https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook)

Note: In group rooms, bots only have access to messages in which they are mentioned. This means that webhooks would only be called when the bot is specifically mentioned in a room (with @).

### 3.2 Construct API requests to create and delete objects using Firepower Device Management (FDM)

So, I'm assuming the branding team mean [Cisco Firepower Device Manager](https://www.cisco.com/c/en/us/products/security/security-management/firepower-device-manager.html). As it says in the sparse URL, FDM "Manages a small-scale Firepower next-generation firewall (NGFW) deployment locally, using the web".

This use case is pretty niche. FDM seems to sit awkwardly between the legacy but widely deployed ASA code and the modern FTD code. [This dude](https://community.cisco.com/t5/network-security/ftd-vs-fmc/td-p/3017936) seems to suggest FDM is only for lower end 5500-X series appliances and certainly not for FP(Firepower) series hardware.

Whilst seemingly not part of this objective - setting up ASA APIs is [here](https://www.cisco.com/c/en/us/td/docs/security/asa/api/qsg-asa-api.html)
Setting up FTD APIs is [here](https://www.cisco.com/c/en/us/td/docs/security/firepower/ftd-api/guide/ftd-rest-api.html)

Now, having reviewing the example code from the DEVCOR Study guide, I'm dissapointed to find this 

```python
    # Settings for the "Firepower Threat Defense REST API" DevNet sandbox
    fdm=FDM_API("10.10.20.65", "admin", "Cisco1234")
```

So we've transposed language. I've spent time unpicking what the hell FDM is and it seems that actually the objective relies on the  Firepower Threat Defense REST API DevNet sandbox. This is annoying from my perspective having gone down the FDM rabbit hole only to find out we are really talking about FTD.

- Obtain an access token to auth your API calls
- Build a JSON payload if needed (unless you're reading data)
- Send a request to the FDM device using one of the HTTP methods and the specific resource URL
- Consume the returned JSON response
- If you make conifugration changes, deploy the changes

Getting an access token should result in the following reponse

```json
{
    "access_token": "eyJhbGciOiJIUzI...553sTnbJUm329A",
    "expires_in": 1800,
    "token_type": "Bearer",
    "refresh_token": "ehbGciOiJIUzI1NiJ...CyfwqwYbNEgIlg",
    "refresh_expires_in": 2400
}
```

### 3.3 Construct API requests using the Meraki platform to accomplish these tasks

- Add new organizations
- Provisioning thousands of new sites in minutes with an automation script
- Automatically onboarding and offboarding new employees devices
- Building your dashboard for store managers, field techs, or unique use cases

Broadly the API contains the same structures as the web dashboard. A single account may own several _organisations_, organisations consist of _networks_, which contain their own _devices_, _SSIDs_ and other specific settings.

| Organisation|             |             |             |            |
| ----------- | ----------- | ----------- | ----------- |----------- |
| Network 1   | Devices     | Clients | SSIDs  | Netflow |
| Network 2   | Devices     | Clients | SSIDs  | Netflow |

To gain API access to Meraki;

- Open the [dashboard.meraki.com](https://dashboard.meraki.com/) URL
- Navigate to Organization | Settings menu
- Ensure that API access is set to "Enable access to the Cisco Meraki Dashboard API"
- Click your username in the top-right corner and go to "My Profile" to generate the API key. You'll only see the key once, so store it in a vault or password manager of your choosing.

That's all lovely, but what you really want is the [Meraki Always-On DevNet sandbox](https://devnetsandbox.cisco.com/RM/Topology)

There are still two versions of the API. The two URLs are:
https://api.meraki.com/api/v0
https://api.meraki.com/api/v1

Note the suffix being v0 and v1.

API calls are limited to five calls per second, per organisation.

### 3.3.A Use the Meraki Dashboard APIs to enable an SSID

You need information to build the change.

- List the organisations and find the organisation ID
- List the networks associated with the OrgID and find the target network ID
- List the available SSID for the network ID and find the target SSID number.
- Get detailed info or upate the SSID configuration by the SSID number.

See enable_meraki_ssid.py in code/

### 3.3.B Use Meraki location APIs to retrieve location data

To enable the location API;

- Open the [dashboard.meraki.com](https://dashboard.meraki.com/) URL
- Navigate to Organization | General menu
- In the "Location and scanning" section, set the Scanning API dropdown box to "Scanning API enabled".
- Record the validator string. 
- Specify a post URL, authentication secret, location API version, and radio type.
- Configure and host your HTTP server to receive JSON objects

Validator is used by Meraki to validate your application: on the first connection, it will check that the server returns the organisation specific validator string as a response, which will verify the orgnisations identity. The Meraki cloud will then begin performing JSON posts.

### 3.4 Construct API calls to retrieve data from Intersight

### 3.5 Construct a Python script using the UCS APIs to provision a new UCS server given a template

### 3.6 Construct a Python script using the Cisco DNA center APIs to retriev and display wireless health information

### 3.7 Describe the capabailities of AppDynamics when instrumenting an application

### 3.8 Describe steps to build a custom dashboard to present data collected from Cisco APIs
