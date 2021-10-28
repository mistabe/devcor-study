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

