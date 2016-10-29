# Errors

> Default screenshot error placeholder images

> <img src="/images/default_large_queued.png" />
> <img src="/images/default_large_error.png" />
> <img src="/images/default_large_limit.png" />
> <img src="/images/default_large_denied.png" />

Screenshot image requests will always return an image as a response even if there was an error with the request. A special image is returned for these conditions so that if it is being displayed to an end user it will still display something and won’t break.

These error images are customizable for each account through the Account Dashboard page.

Error conditions include: processing (visiting and generating the image now), unavailable (target website did not respond, was down, or returned invalid content), account request limits reached, and credential errors.

Status Code | Meaning
----------- | -------
200 | OK -- The request was successful
202 | Accepted -- Your request was successful but is still being processed on the server
400 | Bad Request -- One or more parameters in the request are invalid
401 | Unauthorized -- Your API key/secret key is wrong or the key doesn't have permission
402 | Payment Required -- Your account balance is used up, purchase additional requests in the account dashboard
429 | Too Many Requests -- Too many requests in a short time
500 | Internal Server Error -- There was an issue processing the request, try again
