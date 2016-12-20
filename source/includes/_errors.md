# Errors

The Sense API uses the following error codes:


Error Code | Name | Cause
---------- | ------- | -------
400 | Bad Request | Your request sucks / Request was not valid JSON, or did not pass server-side validation
401 | Unauthorized | Your API key is wrong / Endpoint requires authentication (in the form of JWT token) which thse request did not provide
403 | Forbidden | Token provided by the request does not have the authorization (usually only the owner of the object can edit or delete it)
404 | Not Found | The specified record was not found. Note that this can also be returned if the API route is invalid
405 | Method Not Allowed | The request method (POST, PUT, DELETE etc.) is not accepted on this route
406 | Not Acceptable | You requested a format that isn't json
410 | Gone | The API requested has been removed from our servers
418 | I'm a teapot
429 | Too Many Requests | You're requesting too much! Slow down!
500 | Internal Server Error | We had a problem with our server. Try again later.
503 | Service Unavailable | We're temporarially offline for maintanance. Please try again later.
ss