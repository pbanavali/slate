## Errors

The Kapcharge API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Invalid or expired access token.
403 | Forbidden.
404 | Not Found -- The specified endpoint could not be found.
405 | Method Not Allowed -- You tried to access the endpoint with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The details requested has been removed from our servers.
429 | Too Many Requests -- You reached your quota
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

<br/>