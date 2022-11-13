# Errors

The Status Hero API uses the following error codes:

Code | Meaning
---------- | -------
`400` | Bad Request - Your request is invalid.
`401` | Unauthorized - Your credentials are invalid.
`404` | Not Found - The resource could not be found.
`405` | Method Not Allowed - You tried to access a resource with an invalid method.
`406` | Not Acceptable - You requested a format that isn't JSON.
`410` | Gone - The resource requested has been removed from our servers.
`429` | Too Many Requests - Slow down!
`500` | Internal Server Error - We had a problem with our server.
`503` | Service Unavailable - We're temporarily offline for maintenance.
