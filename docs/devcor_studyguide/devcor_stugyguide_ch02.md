# Cisco DEVCOR Official Certfication Guide

<https://www.ciscopress.com/store/cisco-devnet-professional-devcor-350-901-study-guide-9780137500048>

## Using APIs

Synchronous and Asynchronous is the lingua franca here. Synchronous is more familiar in terms of modernity but asynchronous has certain virtues assuming you've designed your requesting application and service to support parallel request execution and identification of requests using GUIDs etc.

### 2.1 Implement robust REST API error handling for timeouts and rate limits

- Create the request client side
- Transfer the request across the network to the server
- Execute the request at the server
- Transfer the response over the network to the client

In the above stages, you can suffer both network and API errors, indicated by HTTP response codes.

