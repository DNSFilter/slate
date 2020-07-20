---
title: Website Domain API Reference

language_tabs:
  - shell
  - php
  - python

toc_footers:
  - <a href='https://app.webshrinker.com/signup'>Register for a free account</a>
  - <a href='https://www.webshrinker.com/'>Main Webshrinker Site</a>

search: true
---

# Website Domain API Introduction

> Example response for a lookup of webshrinker.com:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "addresses": {
                "ipv4": {
                    "104.25.182.29": [
                        "2016-08-31T00:00:00Z"
                    ],
                    "104.25.183.29": [
                        "2016-10-25T15:27:58Z"
                    ]
                }
            },
            "categories": ["informationtech", "business"],
            "end_date": "2016-11-01",
            "host": "webshrinker.com",
            "language": "en",
            "related": [
                "api.webshrinker.com",
                "docs.webshrinker.com"
            ],
            "start_date": "2016-08-01"
        }
    ]
}
```

The Webshrinker Domain API gives developers the ability to retrieve information about a given domain name, including its main categories, primary language, hosting server IP addresses, known sub-domain names, as well as inbound and outbound hyperlinks.

# Authentication

To make API requests you need to have an access key and secret key. If you don't have an access key yet, you can register for a free account on our [account dashboard](https://app.webshrinker.com/signup).

## Basic HTTP Authentication

> Example HTTP request:

```http
GET /hosts/v2/d2Vic2hyaW5rZXIuY29t HTTP/1.1
Authorization: Basic Yzk1NDJkMDFkNjlmOjE3OTAyYzJjOWIzYQ==
Host: api.webshrinker.com
```

Basic authentication requires that you send your API access and secret key with each request to the service over HTTPS. Most programming frameworks and SDKs support sending Basic HTTP Authentication “out of the box” with a simple function call.

The access key and secret key are used as the “username” and “password” for Basic Authentication.

If you are crafting the "Authorization" header yourself it is the word "Basic" followed by the base64-encoded string "your-access-key:your-secret-key". Additional information about this HTTP header can be found on [Basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) on Wikipedia.

# Making Requests

## Domain  / Hostname Information

> Example domain information request and response for webshrinker.com:

```http
GET /hosts/v2/d2Vic2hyaW5rZXIuY29t HTTP/1.1
Authorization: Basic Yzk1NDJkMDFkNjlmOjE3OTAyYzJjOWIzYQ==
Host: api.webshrinker.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "addresses": {
                "ipv4": {
                    "104.25.182.29": [
                        "2016-08-31T00:00:00Z"
                    ],
                    "104.25.183.29": [
                        "2016-10-25T15:27:58Z"
                    ]
                }
            },
            "categories": ["informationtech", "business"],
            "end_date": "2016-11-01",
            "host": "webshrinker.com",
            "language": "en",
            "related": [
                "api.webshrinker.com",
                "docs.webshrinker.com"
            ],
            "start_date": "2016-08-01"
        }
    ]
}
```

```shell
curl -u "access-key:secret-key" "https://api.webshrinker.com/hosts/v2/d2Vic2hyaW5rZXIuY29t"
```

```php
<?php

$access_key = "<insert your API key>";
$secret_key = "<insert your API secret key>";

$host = "www.webshrinker.com";

// Use URL-safe base64 encoding
$base64_target_host = str_replace(array('+', '/'), array('-', '_'), base64_encode($host));

$api_url = sprintf("https://api.webshrinker.com/hosts/v2/%s", $base64_target_host);

// Initialize the cURL request
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $api_url);
curl_setopt($ch, CURLOPT_USERPWD, "{$access_key}:{$secret_key}");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

$response = json_decode(curl_exec($ch));
$status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

print_r($response);

switch($status_code) {
    case 200:
        // Do something with the JSON response
        break;
    case 400:
        // Bad or malformed HTTP request
        break;
    case 401:
        // Unauthorized
        break;
    case 402:
        // Request limit reached
        break;
}

?>
```

```python
##########################################################################################
# NOTE: If you are using Python 2.7.6 you might run into an issue
# with making API calls using the requests library.
# For a workaround, see:
# http://stackoverflow.com/questions/31649390/python-requests-ssl-handshake-failure
##########################################################################################

import requests
from base64 import urlsafe_b64encode

target_website = b"<the domain name/IP address of the site to retrieve the information about>"

key = "<insert your API key>"
secret_key = "<insert your API secret key>"

api_url = "https://api.webshrinker.com/hosts/v2/%s" % urlsafe_b64encode(target_website).decode('utf-8')

response = requests.get(api_url, auth=(key, secret_key))
status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print(data)
elif status_code == 400:
    # Bad or malformed HTTP request
    print("Bad or malformed HTTP request")
    print(data)
elif status_code == 401:
    # Unauthorized
    print("Unauthorized - check your access and secret key permissions")
    print(data)
elif status_code == 402:
    # Request limit reached
    print("Account request limit reached")
    print(data)
else:
    # General error occurred
    print("A general error occurred, try the request again")
```

This endpoint returns JSON with information about the requested domain name, including its main category, primary language, server IP addresses, and known sub-domain names.

### HTTP Request

`GET https://api.webshrinker.com/hosts/v2/<host>`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
host | true | | The 'host' is part of the request and not an additional parameter. It should be URL safe, Base64-encoded for maximum compatibility. It can be a domain name or IP address.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
start | false | oldest month available | Starting date for search query, format YYYYMMDD (Ex: 20170320)
end | false | most recent month | Ending date for search query, format YYYYMMDD (Ex: 20161225)

<aside class="notice">
The "language" parameter in the HTTP response is the two letter abbreviation of the primary language detected on the website. It returns the two letter ISO 639-1 code for the language. For a list of all ISO 639-1 language codes, see: <a target="_blank" rel="nofollow">https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes</a>
</aside>

## Domain Backlinks

<aside class="notice">
Coming May 2017 - contact us for early access
</aside>

## Domain Outbound Links

<aside class="notice">
Coming May 2017 - contact us for early access
</aside>

# Errors

> Example bad request response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "error": {
        "message": "Invalid start date parameter (use format: YYYYMMDD)"
    }
}
```

Status Code | Meaning
----------- | -------
200 | OK -- The request was successful, data within the start and end date range is returned
400 | Bad Request -- One or more parameters in the request are invalid
401 | Unauthorized -- Your API key/secret key is wrong or the key doesn't have permission
402 | Payment Required -- Your account balance is used up, purchase additional requests in the account dashboard
429 | Too Many Requests -- Too many requests in a short time
500 | Internal Server Error -- There was an issue processing the request, try again
