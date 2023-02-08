# Using the XNET API

This website is intended for people accessing the [XNET](about.md) API's and provides documentation and guidance on how to use it.

All of the available API services are documented in the [API Spec](api-spec.md) page. To learn on how to access it, see below.

!!! Question ""
    XNET is pretty young, so at this stage you'll be experiencing the beginnings of the ecosystem. This means that the API is pretty basic at the moment, but we'll be expanding it further as we go. Feedback, ideas and suggestions are very welcome!

## API Access

The XNET API is accessible through the **`https://services.xnetmobile.com`** endpoint. 

!!! Info ""
    The XNET API's access is restricted to registered users. In case you don't have access yet, make sure to request your API access through our [Discord server](https://discord.com/invite/3W5vTU8aCn).

    Registered users get a unique **`client_id`** and **`client_secret`** to be used to access this API.

Using your credentials, you'll need to get an access token from the XNET Authorization Server before making a request to the XNET API. This is really simple and we provide some examples below.

### Example API Request

As an example to get information about your XNODE, you'd make two requests. The first one to authenticate and the second one to the API service. 

The code snippets below provide examples on getting an overview about your XNODE. These are just example snippets for demonstration purposes, but in case you reuse them, just make sure to set the **`<XNETAPI_XNODE_ID>`**, **`<XNETAPI_CLIENT_ID>`** and **`<XNETAPI_CLIENT_SECRET>`** parameters with your input as OS Environment Variables.

#### Linux shell

``` bash title="getXnodeOverview.sh"
#!/bin/sh

# Collect input parameters from the OS Environment Variables

XNODE_ID="$XNETAPI_XNODE_ID"
if [ -z "$XNODE_ID" ]; then
  echo 'Error: XNETAPI_XNODE_ID environment variable is not set' && exit 1
fi

CLIENT_ID="$XNETAPI_CLIENT_ID"
if [ -z "$CLIENT_ID" ]; then
  echo 'Error: XNETAPI_CLIENT_ID environment variable is not set' && exit 1
fi

CLIENT_SECRET="$XNETAPI_CLIENT_SECRET"
if [ -z "$CLIENT_SECRET" ]; then
  echo 'Error: XNETAPI_CLIENT_SECRET environment variable is not set' && exit 1
fi

# Fixed parameters

AUTH_SERVICE=https://services.xnetmobile.com/v1/auth/token
API_SERVICE="https://services.xnetmobile.com/v1/xsite/${XNODE_ID}"
SCOPE='xnet-external-api'
GRANT_TYPE='client_credentials'

# Request access token from XNET Authorization Server

ACCESS_TOKEN=$(curl -s \
"${AUTH_SERVICE}" \
-d "grant_type=${GRANT_TYPE}" \
-d "scope=${SCOPE}" \
-d "client_id=${CLIENT_ID}" \
-d "client_secret=${CLIENT_SECRET}" | awk -F'"' '{print $4}')

if [ "$ACCESS_TOKEN" = 'none' ]; then
  echo 'Error: Authorization failed. Check your credentials before trying again'
  exit 1
fi

# Request API Service using the access token

curl -i -s \
"${API_SERVICE}" \
-H "Authorization: Bearer ${ACCESS_TOKEN}"
```

#### Typescript

``` typescript title="getXnodeOverview.ts"
// Collect parameter input from the OS Environment Variables
const xnodeId      = process.env.XNETAPI_XNODE_ID;
if (!xnodeId) {
  console.error("Error: XNETAPI_XNODE_ID environment variable is not set.");
  process.exit(1);
}

const clientId     = process.env.XNETAPI_CLIENT_ID;
if (!clientId) {
  console.error("Error: XNETAPI_CLIENT_ID environment variable is not set.");
  process.exit(1);
}

const clientSecret = process.env.XNETAPI_CLIENT_SECRET;
if (!clientSecret) {
  console.error("Error: XNETAPI_CLIENT_SECRET environment variable is not set.");
  process.exit(1);
}

// Fixed parameters
const authService  = 'https://services.xnetmobile.com/v1/auth/token';
const apiService   = 'https://services.xnetmobile.com/v1/xsite/';
const scope        = 'xnet-external-api';
const grant_type   = 'client_credentials';

// Some var initiatilization
let accessToken    = '';
let expiresAt      = 0;

// Request access token from the XNET Authorization Server
async function getAccessToken() {
  try {
    const authResponse = await fetch(authService, {
      method: "POST",
      headers: {
        "Content-Type": "application/x-www-form-urlencoded",
      },
      body: `grant_type=${grant_type}&scope=${scope}&client_id=${clientId}&client_secret=${clientSecret}`,
    });
    const authData = await authResponse.json();
    if (!authResponse.ok) {
      throw new Error(`Authentication failed. Check your credentials before trying again. Status code: ${authResponse.status}`);
    }
    accessToken = authData.access_token;
    expiresAt = Date.now() + authData.expires_in * 1000;
  } catch (error) {
    console.error(error);
  }
}

// Request API Service using the access token
async function getData(xnodeId) {
  let apiServiceUrl=`${apiService}${xnodeId}`
  if (expiresAt < Date.now()) {
    await getAccessToken();
  }

  try {
    const response = await fetch(apiServiceUrl, {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    });
    const data = await response.json();
    console.log(JSON.stringify(data, null, 2));
  } catch (error) {
    console.error(error);
  }
}

getData(xnodeId);
```

#### Python

``` python title="getXnodeOverview.py"
import os
import requests
import json
import time

# Collect input parameters from the OS Environment Variables
xnode_id = os.environ.get("XNETAPI_XNODE_ID")
if not xnode_id:
    print('Error: XNETAPI_XNODE_ID environment variable is not set.')
    exit(1)

client_id = os.environ.get("XNETAPI_CLIENT_ID")
if not client_id:
    print('Error: XNETAPI_CLIENT_ID environment variable is not set.')
    exit(1)

client_secret = os.environ.get("XNETAPI_CLIENT_SECRET")
if not client_secret:
    print('Error: XNETAPI_CLIENT_SECRET environment variable is not set.')
    exit(1)

# Fixed parameters

auth_server = 'https://services.xnetmobile.com/v1/auth/token'
auth_scope = 'xnet-external-api'
auth_grant_type = 'client_credentials'
api_url = 'https://services.xnetmobile.com/v1/xsite/'

# Some var initiatilization

access_token = ""
expires_at = 0

# Request access token from the XNET Authorization Server


def get_access_token():
    global access_token, expires_at
    try:
        auth_response = requests.post(auth_server, data={
            "client_id": client_id,
            "client_secret": client_secret,
            "scope": auth_scope,
            "grant_type": auth_grant_type
        })
        if auth_response.status_code != 200:
            print('Authentication failed. Check your credentials before trying again. Status code: ' + str(auth_response.status_code))
            exit(1)
        auth_data = json.loads(auth_response.text)
        access_token = auth_data["access_token"]
        expires_at = int(auth_data["expires_in"])
    except Exception as error:
        print(f"Error: {error}")

# Request API Service using the access token


def get_data():
    global access_token, expires_at
    if expires_at < int(time.time()):
        get_access_token()
    try:
        api_service_url = api_url + xnode_id
        response = requests.get(api_service_url, headers={
            "Authorization": f"Bearer {access_token}"
        })
        data = json.loads(response.text)
        print(data)
    except Exception as error:
        print(f"Error: {error}")


get_data()
```

#### Go

``` Go title="getXnodeOverview.go"
package main

import (
  "context"
  "fmt"
  "io/ioutil"
  "log"
  "os"

  "golang.org/x/oauth2/clientcredentials"
)

  // Fixed parameters
  const apiUrl = "https://services.xnetmobile.com/v1/xsite/"

  // Var initialization
  var apiServiceUrl = ""

func main() {

  // Collect input parameters from the OS Environment Variables
  xnodeId := os.Getenv("XNETAPI_XNODE_ID")
  if xnodeId == "" {
    log.Fatalf("XNETAPI_XNODE_ID environment variable is not set.")
  }

  clientId := os.Getenv("XNETAPI_CLIENT_ID")
  if clientId == "" {
    log.Fatalf("XNETAPI_CLIENT_ID environment variable is not set.")
  }

  clientSecret := os.Getenv("XNETAPI_CLIENT_SECRET")
  if clientSecret == "" {
    log.Fatalf("XNETAPI_CLIENT_SECRET environment variable is not set.")
  }

  ctx := context.Background()

  // OAuth2.0 Client Credentials Flow configuration
  conf := clientcredentials.Config{
    ClientID:     clientId,
    ClientSecret: clientSecret,
    Scopes:       []string{"xnet-external-api"},
    TokenURL:     "https://services.xnetmobile.com/v1/auth/token",
  }

  // Request access token from the XNET Authorization Server
  _, err := conf.Token(ctx)
  if err != nil {
    log.Fatalf("Authentication failed. Check your credentials before trying again. %v", err)
  }

  // Request API Service using the access token
  apiServiceUrl = apiUrl + xnodeId
  client := conf.Client(ctx)
  resp, err := client.Get(apiServiceUrl)
  if err != nil {
    log.Fatalf("Could not make API call: %v", err)
  }
  defer resp.Body.Close()
  if resp.StatusCode != 200 {
    log.Fatalf("unexpected HTTP status code: %d", resp.StatusCode)
  }
  body, err := ioutil.ReadAll(resp.Body)
  if err != nil {
    log.Fatalf("could not read response body: %v", err)
  }
  fmt.Printf("%s", body)
}
```

### Auth Detail

The detail below might be relevant for you when you're developing your own app which will be using the XNET API.

The XNET API's are authenticated with OAuth 2.0 Authorization Framework  using the Client Credentials flow as per [RFC6749](https://www.rfc-editor.org/rfc/rfc6749#section-4.4). 

     +---------+                                  +---------------+
     |         |                                  |               |
     |         |>--(A)- Client Authentication --->|     XNET      |
     |         |                                  | Authorization |
     |         |<--(B)---- Access Token ---------<|     Server    |
     |         |                                  |               |
     |         |                                  +---------------+
     | Client  |
     |         |                                                      +---------------+
     |         |                                                      |               |
     |         |>--(C)--------- Request with access token ----------->|               |
     |         |                                                      |   XNET API    |
     |         |<--(D)----------------- Response --------------------<|               |
     |         |                                                      |               |
     +---------+                                                      +---------------+
 

**(A)** The first one to get your access token. Step (A) from the diagram above.

    Endoint:               https://services.xnetmobile.com
    HTTP Method:           POST 
    URI:                   /v1/auth/token
    Content-Type:          application/x-www-form-urlencoded
    
    Request payload:
    client_id=<XNETAPI_CLIENT_ID>&client_secret=<XNETAPI_CLIENT_SECRET>&scope=xnet-external-api&grant_type=client_credentials

**(B)** If the authentication is successful, the response will have the following format:

    HTTP Status:           200 OK
    Content-Type:          application/json
    x-ratelimit-limit:     6
    x-ratelimit-remaining: 5   

    Response payload:
    {"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c","expires_in":300,"refresh_expires_in":0,"token_type":"Bearer","not-before-policy":0,"scope":"email xnet-external-api profile"}


**(C)** And then use the resulting **access_token** to request the service from the API.

    Endoint:               https://services.xnetmobile.com
    HTTP Method:           GET 
    URI:                   /v1/xsite/some-xnode-id-1337
    Authorization:         Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

**(D)** The API then responds according to the [API Spec](api-spec.md) for that request.

The API will only provide a successful response if the request is made with a valid access token.

In case an API request is made without the access token or with an invalid one, the API responds with the `401` [HTTP Status Code](https://www.rfc-editor.org/rfc/rfc9110#name-401-unauthorized).

In order to get a successful response from the API, the `Authorization` header must be provided in the following format: 

    Authorization: Bearer <ACCESS_TOKEN>

Where **`<ACCESS_TOKEN>`** is a valid jwt token which has been previously provided by the XNET Authorization Server.

!!! Warning "Token Expiry"
    Do note that the access tokens expire every 300 seconds. New access tokens have to be requested before its expiry. Some packages implement refresh management such as the [oauth2](https://pkg.go.dev/golang.org/x/oauth2/clientcredentials) package in go, for instance (example provided in the go snippet above).

Access tokens must be reused while they're valid. Currently, only 6 auth requests per minute are allowed, while 200 API requests per minute are allowed.

The API responds with **`x-ratelimit-limit`** and **`x-ratelimit-remaining`** HTTP headers.

**`x-ratelimit-limit`** informs about the maximum number of requests per minute

**`x-ratelimit-remaining`** informs about the number of requests per minute that are still allowed
