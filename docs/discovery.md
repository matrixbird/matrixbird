### Discovery

Matrixbird clients can discover the URI of the Matrixbird server by using the
`/.well-known/matrixbird/client` endpoint. This endpoint should return a JSON object
containing the server's URI, along with additional optional data.

The endpoint can be accessed using a HTTP GET request and should return a
response like the following:

```json
{
  "matrixbird.server": {
    "url": "https://matrixbird.matrixbird.com"
    "related": [
      "matrixbird.net",
      "matrixbird.org"
    ],
  },
  "m.homeserver": {
    "base_url": "https://matrix.matrixbird.com",
    "server_name": "matrixbird.com"
  }
}
```

In the example above, the `server.url` field contains the URI of the Matrixbird
server, and the `related` field contains a list of related matrixbird servers
that the client can also connect to. The `m.homeserver` field is optional since
this is already served by `/.well-known/matrixbird/client`, but it's useful to
save an extra request.

Matrixbird servers communicating with other Matrixbird servers over federation
should use the `/.well-known/matrixbird/server` endpoint to discover the URI
of the server. This endpoint should also return a JSON object containing the
server's URI. The response should look like this:

```json
{
  "matrixbird.server": {
    "url": "https://matrixbird.matrixbird.com"
  }
}
```

Additional optional data can be included in the response, such as verifying keys
for server-to-server communication, supported features, or other metadata.


> [!WARNING]  
> The mechanism for discovery is only an initial proposal and may change in the
> future.



