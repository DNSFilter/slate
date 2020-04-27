---
title: Website Domain API Reference

language_tabs:
  - shell
  - php
  - python

toc_footers:
  - <a href='https://dashboard.webshrinker.com/register'>Register for a free account</a>
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
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "language": "en",
            "categories": [
                "business",
                "informationtech"
            ],
            "host": "webshrinker.com",
            "related": [
                "docs.webshrinker.com",
                "mautic.webshrinker.com",
                "dashboard.webshrinker.com"
            ],
            "addresses": {
                "ipv4": {
                    "104.25.241.16": [
                        "2017-07-18T21:44:33Z"
                    ],
                    "64.90.40.82": [
                        "2017-07-12T03:17:17Z"
                    ],
                    "104.25.183.29": [
                        "2016-10-25T15:27:58Z"
                    ],
                    "104.25.182.29": [
                        "2016-08-31T00:00:00Z"
                    ]
                }
            }
        }
    ],
    "paging": {
        "cursors": {},
        "next": "",
        "count": 7
    }
}
```

The Webshrinker Domain API gives developers the ability to retrieve information about a given domain name, including its main categories, primary language, hosting server IP addresses, known sub-domain names, as well as inbound and outbound hyperlinks.

# Authentication

To make API requests you need to have an access key and secret key. If you don't have an access key yet, you can register for a free account on our [account dashboard](https://dashboard.webshrinker.com/register).

## Basic HTTP Authentication

> Example HTTP request:

```http
GET /hosts/v3/d2Vic2hyaW5rZXIuY29t HTTP/1.1
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
GET /hosts/v3/d2Vic2hyaW5rZXIuY29t HTTP/1.1
Authorization: Basic Yzk1NDJkMDFkNjlmOjE3OTAyYzJjOWIzYQ==
Host: api.webshrinker.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "language": "en",
            "categories": [
                "business",
                "informationtech"
            ],
            "host": "webshrinker.com",
            "related": [
                "docs.webshrinker.com",
                "mautic.webshrinker.com",
                "dashboard.webshrinker.com"
            ],
            "addresses": {
                "ipv4": {
                    "104.25.241.16": [
                        "2017-07-18T21:44:33Z"
                    ],
                    "64.90.40.82": [
                        "2017-07-12T03:17:17Z"
                    ],
                    "104.25.183.29": [
                        "2016-10-25T15:27:58Z"
                    ],
                    "104.25.182.29": [
                        "2016-08-31T00:00:00Z"
                    ]
                }
            }
        }
    ],
    "paging": {
        "cursors": {},
        "next": "",
        "count": 7
    }
}
```

```shell
curl -u "access-key:secret-key" "https://api.webshrinker.com/hosts/v3/d2Vic2hyaW5rZXIuY29t"
```

```php
<?php

$access_key = "<insert your API key>";
$secret_key = "<insert your API secret key>";

$host = "www.webshrinker.com";

// Use URL-safe base64 encoding
$base64_target_host = str_replace(array('+', '/'), array('-', '_'), base64_encode($host));

$api_url = sprintf("https://api.webshrinker.com/hosts/v3/%s", $base64_target_host);

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

api_url = "https://api.webshrinker.com/hosts/v3/%s" % urlsafe_b64encode(target_website).decode('utf-8')

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

`GET https://api.webshrinker.com/hosts/v3/<host>`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
host | true | | The 'host' is part of the request and not an additional parameter. It should be URL safe, Base64-encoded for maximum compatibility. It can be a domain name or IP address.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
start | false | oldest month available | Starting date for search query, format YYYYMMDD (Ex: 20170320)
end | false | most recent month | Ending date for search query, format YYYYMMDD (Ex: 20161225)
limit | false | 3000 | Limit the number of results to this number, max value: 3000. This is the total number of entries for both 'addresses' and 'related' fields returned in one response.
sort | false | desc | Best effort attempt to order results in ascending (asc) or descending (desc) order. Default is 'desc' (newer -> older).

<aside class="notice">
The "language" parameter in the HTTP response is the two letter abbreviation of the primary language detected on the website. It returns the two letter ISO 639-1 code for the language. For a list of all ISO 639-1 language codes, see: <a target="_blank" rel="nofollow">https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes</a>
</aside>

## Domain Backlinks

> Sample backlinks for example.com with a limit of 2

```http
GET /hosts/v3/ZXhhbXBsZS5jb20=/links/inbound?limit=2 HTTP/1.1
Host: api.webshrinker.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "links": [
                {
                    "from": "https://metalab.at/wiki/index.php?title=metaday_30&diff=30727&oldid=30726",
                    "to": "http://example.com/",
                    "seen": "2016-10-23T23:17:16Z",
                    "attrs": {
                        "text": "foo"
                    }
                },
                {
                    "from": "http://blog.youaresecure.be/",
                    "to": "http://example.com/",
                    "seen": "2016-10-23T21:49:12Z",
                    "attrs": {
                        "text": "link to example"
                    }
                }
            ]
        }
    ],
    "paging": {
        "cursors": {
            "after": "B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8LWAMFUAVXAFUEC1NSXAlRBABUBlIDV1gHAQEAAFULVwBb"
        },
        "next": "https://api.webshrinker.com/hosts/v3/ZXhhbXBsZS5jb20=/links/inbound?limit=2&page=B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8LWAMFUAVXAFUEC1NSXAlRBABUBlIDV1gHAQEAAFULVwBb",
        "count": 2,
        "remaining": 1998
    }
}
```

```php
<?php

$access_key = "<insert your API key>";
$secret_key = "<insert your API secret key>";

$host = "www.webshrinker.com";

// Use URL-safe base64 encoding
$base64_target_host = str_replace(array('+', '/'), array('-', '_'), base64_encode($host));

$params = array(
    'limit' => 10
);

$api_url = sprintf("https://api.webshrinker.com/hosts/v3/%s/links/inbound?%s", $base64_target_host, http_build_query($params));

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
import json
from base64 import urlsafe_b64encode

try:
    from urllib import urlencode
except ImportError:
    from urllib.parse import urlencode

target_website = b"<the domain name/IP address of the site to retrieve the hyperlinks>"

key = "<insert your API key>"
secret_key = "<insert your API secret key>"

params = {
    "limit": 10
}

api_url = "https://api.webshrinker.com/hosts/v3/{}/links/inbound?{}".format(urlsafe_b64encode(target_website).decode('utf-8'), urlencode(params, True))

response = requests.get(api_url, auth=(key, secret_key))
status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print(json.dumps(data, indent=4))
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

The domain backlinks (or inbound links) allows you to search through all links we've seen which are pointing to the target domain. You can narrow your search by using start/end dates and access attributes such as "rel", "text", "alt", and "title" for each hyperlink.

### HTTP Request

`GET https://api.webshrinker.com/hosts/v3/<host>/links/inbound`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
host | true | | The 'host' is part of the request and not an additional parameter. It should be URL safe, Base64-encoded for maximum compatibility. It can be a domain name or IP address.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
start | false | oldest month available | Starting date for search query, format YYYYMMDD (Ex: 20170320)
end | false | most recent month | Ending date for search query, format YYYYMMDD (Ex: 20161225)
limit | false | 3000 | Limit the number of results to this number, max value: 3000
subdomains | false | false | By default links are returned for the queried host only. If you want to include subdomains of that host, set this to true.
sort | false | desc | Best effort attempt to order results in ascending (asc) or descending (desc) order. Default is 'desc' (newer -> older).

## Domain Outbound Links

> Sample outbound links for example.com

```http
GET /hosts/v3/ZXhhbXBsZS5jb20=/links/outbound HTTP/1.1
Host: api.webshrinker.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "links": [
                {
                    "from": "http://example.com/",
                    "to": "http://www.iana.org/domains/example",
                    "seen": "2017-07-08T06:19:17Z",
                    "attrs": {
                        "text": "More information..."
                    }
                }
            ]
        }
    ],
    "paging": {
        "cursors": {},
        "next": "",
        "count": 1,
        "remaining": 0
    }
}
```

```php
<?php

$access_key = "<insert your API key>";
$secret_key = "<insert your API secret key>";

$host = "www.example.com";

// Use URL-safe base64 encoding
$base64_target_host = str_replace(array('+', '/'), array('-', '_'), base64_encode($host));

$params = array(
    'limit' => 10
);

$api_url = sprintf("https://api.webshrinker.com/hosts/v3/%s/links/outbound?%s", $base64_target_host, http_build_query($params));

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
import json
from base64 import urlsafe_b64encode

try:
    from urllib import urlencode
except ImportError:
    from urllib.parse import urlencode

target_website = b"<the domain name/IP address of the site to retrieve outbound hyperlinks>"

key = "<insert your API key>"
secret_key = "<insert your API secret key>"

params = {
    "limit": 10
}

api_url = "https://api.webshrinker.com/hosts/v3/{}/links/outbound?{}".format(urlsafe_b64encode(target_website).decode('utf-8'), urlencode(params, True))

response = requests.get(api_url, auth=(key, secret_key))
status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print(json.dumps(data, indent=4))
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

The domain outbound links allow you to search through all hyperlinks we've seen on the target site that point to another destination. 
You can narrow your search by using start/end dates and access attributes such as "rel", "text", "alt", and "title" for each hyperlink.

### HTTP Request

`GET https://api.webshrinker.com/hosts/v3/<host>/links/outbound`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
host | true | | The 'host' is part of the request and not an additional parameter. It should be URL safe, Base64-encoded for maximum compatibility. It can be a domain name or IP address.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
start | false | oldest month available | Starting date for search query, format YYYYMMDD (Ex: 20170320)
end | false | most recent month | Ending date for search query, format YYYYMMDD (Ex: 20161225)
limit | false | 3000 | Limit the number of results to this number, max value: 3000
subdomains | false | false | By default links are returned for the queried host only. If you want to include subdomains of that host, set this to true.
sort | false | desc | Best effort attempt to order results in ascending (asc) or descending (desc) order. Default is 'desc' (newer -> older).

# Understanding the Response

## Paging

> Sample backlinks for "example.com" with paging and a limit of 1 result

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "links": [
                {
                    "from": "https://metalab.at/wiki/index.php?title=metaday_30&diff=30727&oldid=30726",
                    "to": "http://example.com/",
                    "seen": "2016-10-23T23:17:16Z",
                    "attrs": {
                        "text": "foo"
                    }
                }
            ]
        }
    ],
    "paging": {
        "cursors": {
            "after": "B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8IWFJXBwBTD1MJUwkGWwtTAwZQVAcEAgJVDVJVBQxRVA8D"
        },
        "next": "https://api.webshrinker.com/hosts/v3/ZXhhbXBsZS5jb20=/links/inbound?limit=1&page=B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8IWFJXBwBTD1MJUwkGWwtTAwZQVAcEAgJVDVJVBQxRVA8D",
        "count": 1,
        "remaining": 1999
    }
}
```

> Sample "paging" attribute when there are no more results

```json
{
    "paging": {
        "cursors": {},
        "next": "",
        "count": 7
    }
}
```
When there are more results available than what was returned, you will need to use the paging feature of the API. Paging allows you to execute the same query but retrieve the next set of results. 
With paging, it's possible to break up very large result sets into smaller, more manageable pieces.

Consider the example on the side as it shows backlinks for "example.com" with a result limit of 1. The "paging" attribute says there are remaining entries and provides an "after" cursor to access them. 
To use the cursor, make the same request to the API but add a query parameter called "page" with the value of the cursor. For example:

`https://api.webshrinker.com/hosts/v3/ZXhhbXBsZS5jb20=/links/inbound?page=B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQ`

The "after" cursor will have a different value after each query. You will need to use the latest "after" cursor value to access the next set of results. An alternative to using the "after" cursor 
is to use the "next" attribute. This attribute is the full URL to access and it includes all parameters to make the same request but for the next set of results.

### Paging Fields

Field | Description
----- | -----------
cursors / after | If not empty, this is the value to pass to the next API query as the "page" parameter to get the next set of results
next | The full API URL to request to access the next set of results (just include your access credentials)
count | The number of results returned
remaining | Estimated number of entries left to retrieve

## Hyperlink Data

> Sample backlinks for "example.com"

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "start_date": "2016-08-01",
            "end_date": "2017-10-31",
            "links": [
                {
                    "from": "https://somewhere.at/wiki/index.php?title=metaday_30&diff=30727&oldid=30726",
                    "to": "http://example.com/",
                    "seen": "2016-10-23T23:17:16Z",
                    "attrs": {
                        "text": "foo",
                        "target": "_blank",
                        "rel": "nofollow"
                    }
                }
            ]
        }
    ],
    "paging": {
        "cursors": {
            "after": "B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8IWFJXBwBTD1MJUwkGWwtTAwZQVAcEAgJVDVJVBQxRVA8D"
        },
        "next": "https://api.webshrinker.com/hosts/v3/ZXhhbXBsZS5jb20=/links/inbound?limit=1&page=B04CCUJYBEtXDAwIUVYDWgMGBFYEVgMBCVRUXQgKBlYGAgJSUQAHUgZQAAIOAw8IWFJXBwBTD1MJUwkGWwtTAwZQVAcEAgJVDVJVBQxRVA8D",
        "count": 1,
        "remaining": 1999
    }
}
```

### Link Fields

Field | Description
----- | -----------
from | The URL where the hyperlink was discovered.
to | The target URL, where the hyperlink points.
seen | The ISO 8601 date when the link was discovered.
attrs | Data about the link


### "attrs" Fields

Field | Description
----- | -----------
text | The text shown to the user for the link
target | The target for the link
rel | Any rel tags, separated by commas (example: nofollow,noopener)
title | The HTML title attribute for the link
alt | Alternate text (normally for assistive technologies like text readers)

<aside class="notice">
These attributes will only be present if they were originally seen on the hyperlink.
</aside>

# Errors

> Example bad request response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "message": "Invalid start date parameter (use format: YYYYMMDD)"
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
