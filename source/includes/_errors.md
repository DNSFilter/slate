# Errors

Error Code | Meaning
---------- | -------
200 | OK -- The request was successful
202 | Accepted -- Your request was successful but is still being processed on the server
400 | Bad Request -- One or more parameters in the request are invalid
401 | Unauthorized -- Your API key/secret key is wrong or the key doesn't have permission
402 | Payment Required -- Your account balance is used up, purchase additional requests in the account dashboard
429 | Too Many Requests -- Too many requests in a short time
500 | Internal Server Error -- There was an issue processing the request, try again
