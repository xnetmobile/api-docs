# API Spec

## Get Access Token

### Request
``` 
POST https://services.xnetmobile.com/v1/auth/token
```

### Responses

#### 200 OK

``` json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
  "expires_in": 300,
  "refresh_expires_in": 0,
  "token_type": "Bearer",
  "not-before-policy": 0,
  "scope": "email xnet-external-api profile"
}
```

#### 401 Unauthorized
Wrong or invalid input has been provided. Check [Using the XNET API](index.md)

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](index.md)

- - -

## Get List of All XNODE IDs
### Request

    GET https://services.xnetmobile.com/v1/xsite/xnodes

### Responses

#### 200 OK

``` json
{
  "request": {
    "schema": "1.0",
    "request": "/v1/xsite/xnodes",
    "timestamp": 1665340858
  },
  "count": 4,
  "xnodeids": [
    "dragon-ball-original-1986",
    "dragon-ball-z-1989",
    "dragon-ball-gt-1996",
    "dragon-ball-super-2015"
  ]
}
```
#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](index.md)

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](index.md)

- - -

## XSITE Information

### Request

    GET https://services.xnetmobile.com/v1/xsite/{xnodeId}

#### Description 
Where `{xnodeId}` is an XNODE ID.

### Responses

#### 200 OK

``` json 
{
  "request": {
    "schema": "1.0",
    "request": "/v1/xsite/dragon-ball-super-2015",
    "timestamp": "1675801741"
  },
  "xsite": {
    "xsiteid": "afa1d822-6b3d-4a15-86eb-d5ba1dbf9e81",
    "ethwallet": {
      "ethaddress": "0xd0C75df73a5fd87a35fd8a3765dfaf36d5fa6785",
      "ethpublickey": "0x04ebda27ea32514ef3a51ef3a5421efa5421efa542e13af5124ef3a512e43fa512e3f5124e3af5124efa123e6851348613461854afa6f36152fa36125f43a32123"
    },
    "xnode": {
      "count": 1,
      "xnodes": [
        {
          "xnodeid": "dragon-ball-super-2015",
          "macaddress": "00:ca:fe:be:ef:00",
          "hardware": "namek",
          "firmware": "piccolo",
          "provisioned": true,
          "registered": true,
          "status": {
            "state": "up",
            "lastseen": 1675801705,
            "lastupdated": 1675760820
          }
        }
      ]
    },
    "radio": {
      "count": 1,
      "radios": [
        {
          "radioid": "ac2be1e0-33ac-45ec-89d9-f7a785c15004",
          "xnodeid": "dragon-ball-super-2015",
          "macaddress": "00:be:ef:ca:fe:00",
          "vendorname": "SomeRadioVendor",
          "vendormodel": "Nimbus",
          "vendorfirmware": "kakarot_2.1",
          "vendorsn": "3216487614FDS13",
          "provisioned": true,
          "registered": true,
          "status": {
            "state": "up",
            "lastseen": 1675801708,
            "location": {
              "lat": "12.123456",
              "lng": "-123.123456",
              "aglm": "NaN"
            }
          }
        }
      ]
    }
  }
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](index.md)

#### 404 Not Found
The specified `{xnodeId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](index.md)

- - -

