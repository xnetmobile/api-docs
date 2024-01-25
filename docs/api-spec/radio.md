# Radio

## Get List of Radios
Returns a list of active Radio IDs.
### Request

    GET https://api.xnetmobile.com/xsite/radio?limit=4&page=1

### Responses

#### 200 OK

``` json
{
  "radioids": [
    "c6b66ae9-602d-432b-afbe-ab2932c6881a",
    "7f953de3-767b-4bb0-b74e-6ea3e763fb3c",
    "c1dc9c85-a283-4238-8889-8b3ab988d0b2",
    "1443b851-d2bb-4147-a896-d40a8a55f2f1"
  ],
  "total_count": 12345,
  "total_pages": 3087,
  "current_page": 1,
  "limit": 4
}
```
#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -

## Radio Information
Returns information about a particular Radio.

### Request

    GET https://api.xnetmobile.com/xsite/radio/{radioId}

#### Description 
Where `{radioId}` is a Radio ID.

### Responses

#### 200 OK

``` json 
{
  "radioid": "c1dc9c85-a283-4238-8889-8b3ab988d0b2",
  "xnodeid": "dragon-ball-super-2015",
  "macaddress": "12:34:56:78:90:ab",
  "radiomodelid": "29583972-c207-4b72-8948-b10cb7c93b88",
  "vendorname": "Baicells",
  "vendormodel": "pBS3101S",
  "vendorfirmware": "BaiBS_QRTB_2.12.8",
  "vendorsn": "120200000000AGB0123",
  "synctype": "gnss",
  "notes": "",
  "provisioned": true,
  "registered": true,
  "uptime": 12345,
  "status": {
    "xnodeid": "dragon-ball-super-2015",
    "lastseen": 1706184964,
    "state": "up",
    "location": {
      "latitude": "37.553312",
      "longitude": "-122.307777",
      "altitude": "NaN"
    }
  }
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{radioId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -

## Radio Reports
Returns status reports from a particular Radio.

### Request

    GET https://api.xnetmobile.com/xsite/radio/{radioId}/report?limit=2&page=1

#### Description 
Where `{radioId}` is a Radio ID.

### Responses

#### 200 OK

``` json 
{
  "reports": [
    {
      "xnodeid": "dragon-ball-super-2015",
      "lastseen": 1706184964,
      "state": "up",
      "location": {
        "latitude": "37.553312",
        "longitude": "-122.307777",
        "altitude": "NaN"
      }
    },
    {
      "xnodeid": "dragon-ball-super-2015",
      "lastseen": 1706184349,
      "state": "faulty",
      "location": {
        "latitude": "37.553301",
        "longitude": "-122.307787",
        "altitude": "NaN"
      }
    }
  ],
  "total_count": 12345,
  "total_pages": 6173,
  "current_page": 1,
  "limit": 2
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{radioId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -