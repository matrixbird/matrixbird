### Discovery

Matrixbird clients can discover the URI of the Matrixbird server by using the
`/.well-known/matrixbird` endpoint. This endpoint should return a JSON object
containing the server's URI.

The endpoint can be accessed using a HTTP GET request and should return a
response like the following:

```json
{
  "server": {
    "url": "https://api.matrixbird.com"
  }
}
```

Matrixbird servers communicating with each other can also use the same
`/.well-known/matrixbird` endpoint to discover each other's URIs. This may
change in the future if it becomes necessary to separate discovery for clients
and servers.
