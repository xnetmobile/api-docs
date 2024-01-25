# Operator

## Get List of Operators
Returns a list of Operator IDs.
### Request

    GET https://api.xnetmobile.com/xsite/operator?limit=3&page=1

### Responses

#### 200 OK

``` json
{
  "operatorids": [
    "2e31979-2228-4fd2-a430-36970f852dc4",
    "e737b70f-9be1-4a01-aefb-a3623a829d7b",
    "af05aff1-08aa-4e34-930f-5e8dd4f50cc7",
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

## Operator Information
Returns information about a particular Operator.

### Request

    GET https://api.xnetmobile.com/xsite/operator/{operatorId}

#### Description 
Where `{operatorId}` is an Operator ID.

### Responses

#### 200 OK

``` json 
{
  "operatorid": "e737b70f-9be1-4a01-aefb-a3623a829d7b",
  "name": "John Doe",
  "isnew": false,
  "address": "400 Concar Dr - San Mateo - CA 94402",
  "phone": "1234567890",
  "email": "john@example.com",
  "wallet": "0x1a2b3c3d4e5f1g2h3i4j5",
  "user_discord": "john",
  "user_telegram": "john"
}
```

#### 401 Unauthorized
Access token is invalid or has not been provided. Check [Using the XNET API](../index.md)

#### 404 Not Found
The specified `{operatorId}` was not found

#### 503 Service Unavailable
Rate limit has been exceeded. You may consult the `x-ratelimit-limit` and `x-ratelimit-remaining` headers. Check [Using the XNET API](../index.md)

- - -