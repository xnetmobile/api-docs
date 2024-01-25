# XNODE

## Get List of XNODEs
Returns a list of active XNODE IDs.
### Request

    GET https://api.xnetmobile.com/xsite/xnode?limit=4&page=1

### Responses

#### 200 OK

``` json
{
  "xnodeids": [
    "dragon-ball-original-1986",
    "dragon-ball-z-1989",
    "dragon-ball-gt-1996",
    "dragon-ball-super-2015"
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

## XNODE Information
Returns information about a particular XNODE.

### Request

    GET https://api.xnetmobile.com/xsite/xnode/{xnodeId}

#### Description 
Where `{xnodeId}` is a XNODE ID.

### Responses

#### 200 OK

``` json 
{
  "xnodeid": "dragon-ball-super-2015",
  "siteid": "d5d971dd-3a67-4c03-a6f5-ec427c117c43",
  "wallet": {
    "ethaddress": "0xd0C75df73a5fd87a35fd8a3765dfaf36d5fa6785",
    "ethpublickey": "0x04ebda27ea32514ef3a51ef3a5421efa5421efa542e13af5124ef3a512e43fa512e3f5124e3af5124efa123e6851348613461854afa6f36152fa36125f43a32123"
  },
  "macaddress": "00:ca:fe:be:ef:00",
  "hardware": "namek",
  "firmware": "piccolo",
  "provisioned": true,
  "registered": true,
  "uptime": 311,
  "status": {
    "lastseen": 1675801705,
    "state": "up",
    "lastupdated": 1675760820
  }
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{xnodeId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -

## XNODE Reports
Returns status reports from a particular XNODE.

### Request

    GET https://api.xnetmobile.com/xsite/xnode/{xnodeId}/report?limit=4&page=1

#### Description 
Where `{xnodeId}` is a XNODE ID.

### Responses

#### 200 OK

``` json 
{
  "reports": [
    {
      "lastseen": 1705874571,
      "state": "up",
      "lastupdated": 1705827669
    },
    {
      "lastseen": 1705874259,
      "state": "up",
      "lastupdated": 1705827669
    },
    {
      "lastseen": 1705873260,
      "state": "faulty",
      "lastupdated": 1705827669
    },
    {
      "lastseen": 1705872641,
      "state": "up",
      "lastupdated": 1705827669
    }
  ],
  "total_count": 682,
  "total_pages": 171,
  "current_page": 1,
  "limit": 4
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{xnodeId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -