# Site

## Get List of Sites
Returns a list of Site IDs.
### Request

    GET https://api.xnetmobile.com/xsite/site?limit=3&page=1

### Responses

#### 200 OK

``` json
{
  "siteids": [
    "3e31979-2228-4fd2-a430-36970f852dc5",
    "f737b70f-9be1-4a01-aefb-a3623a829d7c",
    "bf05aff1-08aa-4e34-930f-5e8dd4f50cc8",
  ],
  "total_count": 1234,
  "total_pages": 412,
  "current_page": 1,
  "limit": 3
}
```
#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -

## Site Information
Returns information about a particular Site.

### Request

    GET https://api.xnetmobile.com/xsite/site/{siteId}

#### Description 
Where `{siteId}` is an Site ID.

### Responses

#### 200 OK

``` json 
{
  "siteid": "f737b70f-9be1-4a01-aefb-a3623a829d7c",
  "operatorid": "e737b70f-9be1-4a01-aefb-a3623a829d7b",
  "address": "400 Concar Dr - San Mateo - CA 94402",
  "opscontact": "123456789",
  "latitude": 37.553312,
  "longitude": -122.307777,
  "altitude": 45,
  "hex": 0,
  "sasapproved": true,
  "submitted": true,
  "submitdate": "2023-01-01T00:00:00Z"
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{siteId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -