# Cisco DEVCOR 350-901 Study Guide

<https://www.ciscopress.com/store/cisco-devnet-professional-devcor-350-901-study-guide-9780137500048>

## 3.0 Cisco Platforms

Interacting with Cisco products and their respective APIs is the body of this chapter.
The [Cisco DevNet Sandbox](https://developer.cisco.com/site/sandbox/) will assist here greatly so make sure you've an account which can access the sandbox for furnishing your journey.
You'll also need an account at the [Cisco Webex Developer](https://developer.webex.com/) portal

### 3.1 Construct API requests to implement ChatOps with Webex API

Webex Teams was renamed to Webex. Brilliant. The API service URLs we'll use will be the new URLs.

I'll correlate as much of this data as possible to using Slack APIs and reference them alongside the Webex APIs.

Chatbots help operations by simulating conversations. Chatbots can respond to question or even start automated workflows.

- Notifiers
- Controllers
- Personal Assistants

With Webex chatbots, the chatbot needs to listen to a webhook URL for Webex notifications
[Slack bot link](https://api.slack.com/bot-users)
Practically speaking it's a small web server that is serving HTTP requests and has to be publicly available.

- Webex access token
- Webhook listener (Python Flask module)
- HTTP Library
- Authetication Credentials (Meraki API key for example)

[Ngrok](https://ngrok.com/) solves(!) the problem of publicly reachable URLs for a private location.
Your webhook [has to be registered](https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook) with the API Call

Webhooks that are created have to be registered, documnetation on how to do that [here](https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook)

### 3.2 Construct API requests to create and delete objects using Firepower Device Management (FDM)

- Obtain an access token to auth your API calls
- Build a JSON payload if needed (unless you're reading data)
- Send a request to the FDM device using one of the HTTP methods and the specific resource URL
- Consume the returned JSON response
- If you make conifugration changes, deploy the changes

FDM is a component of FTD - perhaps the replacement to ASDM for ASA.

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