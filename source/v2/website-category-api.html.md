---
title: Website Category API Reference

language_tabs:
  - shell
  - php
  - python

toc_footers:
  - <a href='https://dashboard.webshrinker.com/register'>Register for a free account</a>
  - <a href='https://www.webshrinker.com/'>Main Web Shrinker Site</a>

search: true
---

# Website Category API Introduction

> Example JSON response for a category lookup of webshrinker.com:

> <a target="_blank" href="https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3">https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3</a>

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "categories": [
                "informationtech",
                "business"
            ],
            "url": "webshrinker.com"
        }
    ]
}
```

The Web Shrinker Category API gives developers the ability to lookup the categories that a particular URL, website, domain name, or IP address is categorized as.

# Authentication

To make API requests you need to have an access key and secret key. If you don't have an access key yet, you can register for a free account on our [account dashboard](https://dashboard.webshrinker.com/register).

There are two methods of authentication that can be used to make API requests: Basic HTTP Authentication and Pre-signed URLs.

## Basic HTTP Authentication

> Example HTTP request:

```http
GET /categories/v2/d2Vic2hyaW5rZXIuY29t HTTP/1.1
Authorization: Basic Yzk1NDJkMDFkNjlmOjE3OTAyYzJjOWIzYQ==
Host: api.webshrinker.com
```

Basic authentication requires that you send your API access and secret key with each request to the service over HTTPS. Most programming frameworks and SDKs support sending Basic HTTP Authentication “out of the box” with a simple function call.

The access key and secret key are used as the “username” and “password” for Basic Authentication.

If you are crafting the "Authorization" header yourself it is the word "Basic" followed by the base64-encoded string "your-access-key:your-secret-key". Additional information about this HTTP header can be found on [Basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) on Wikipedia.

## Pre-signed URLs

> Example pre-signed category lookup:

> <a target="_blank" href="https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3">https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3</a>

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "categories": [
                "informationtech",
                "business"
            ],
            "url": "webshrinker.com"
        }
    ]
}
```

Generating a pre-signed URL allows you to make requests without revealing your secret key. It’s perfect for situations where you need to embed a request in an application or in a webpage but don’t want users to know your secret key, preventing a third party from making unauthorized requests against your account.

The URL used to make the API request is signed using your access and secret key but the URL itself doesn’t contain the secret key. Instead it contains an extra parameter called "hash".

The "hash" is the MD5 hash of your secret key, a colon (":"), and the request URL with query parameters. 

```shell
WS_ACCESS_KEY="your access key"
WS_SECRET_KEY="your secret key"

URL=($(echo -n "https://www.webshrinker.com" | base64))

REQUEST="categories/v2/$URL?key=$WS_ACCESS_KEY"
HASH=($(echo -n "$WS_SECRET_KEY:$REQUEST" | (md5sum || md5)))

echo "https://api.webshrinker.com/$REQUEST&hash=$HASH"
```

```php
<?php

function webshrinker_categories_v2($access_key, $secret_key, $url="", $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("categories/v2/%s?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$url = "https://www.webshrinker.com/";
$signedUrl = webshrinker_categories_v2($access_key, $secret_key, $url);

echo "$signedUrl\n";

?>
```

```python
from urllib import urlencode
from base64 import b64encode
import hashlib

def webshrinker_categories_v2(access_key, secret_key, url="", params={}):
    params['key'] = access_key

    request = "categories/v2/%s?%s" % (b64encode(url), urlencode(params, True))
    signed_request = hashlib.md5("%s:%s" % (secret_key, request)).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

url = "https://www.webshrinker.com/"
print webshrinker_categories_v2(access_key, secret_key, url)

```

# Making Requests

## Category List

> Example category list JSON response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "categories": [
                "uncategorized",
                "searchenginesandportals",
                "newsandmedia",
                "streamingmedia",
                "entertainment",
                "shopping",
                "vehicles",
                "gambling",
                "informationtech",
                "games",
                "sports",
                "economyandfinance",
                "jobrelated",
                "hacking",
                "messageboardsandforums",
                "socialnetworking",
                "chatandmessaging",
                "mediasharing",
                "blogsandpersonal",
                "health",
                "adult",
                "personals",
                "religion",
                "travel",
                "abortion",
                "education",
                "drugs",
                "alcoholandtobacco",
                "business",
                "advertising",
                "humor",
                "foodandrecipes",
                "realestate",
                "weapons",
                "proxyandfilteravoidance",
                "virtualreality",
                "translators",
                "parked",
                "illegalcontent"
            ]
        }
    ]
}
```

```shell
curl -u "access-key:secret-key" "https://api.webshrinker.com/categories/v2"

or

curl "https://api.webshrinker.com/categories/v2?key=NB6E8ZYEYTo8brr73dmT&hash=f4512f0a258b8e39fdf623c4b99e9915"
```

```php
<?php

function webshrinker_categories_v2($access_key, $secret_key, $url="", $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("categories/v2/%s?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$request = webshrinker_categories_v2($access_key, $secret_key);

// Initialize cURL and use pre-signed URL authentication
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $request);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

$response = curl_exec($ch);
$status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

print_r(json_decode($response));

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

from urllib import urlencode
from base64 import b64encode
import hashlib
import requests

def webshrinker_categories_v2(access_key, secret_key, url="", params={}):
    params['key'] = access_key

    request = "categories/v2/%s?%s" % (b64encode(url), urlencode(params, True))
    signed_request = hashlib.md5("%s:%s" % (secret_key, request)).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

api_url = webshrinker_categories_v2(access_key, secret_key)
response = requests.get(api_url)

status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print data
elif status_code == 202:
    # The website is being visited and the categories will be updated shortly
    print data
elif status_code == 400:
    # Bad or malformed HTTP request
    print "Bad or malformed HTTP request"
    print data
elif status_code == 401:
    # Unauthorized
    print "Unauthorized - check your access and secret key permissions"
    print data
elif status_code == 402:
    # Request limit reached
    print "Account request limit reached"
    print data
else:
    # General error occurred
    print "A general error occurred, try the request again"
```

This endpoint returns JSON with all of the possible categories that URLs, hostnames, and IP addresses can be associated with.

### HTTP Request

`GET https://api.webshrinker.com/categories/v2`

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
key | true (if using Pre-signed URLs) | | Your account access key to use for the request.
expires | false | 0 | A unix timestamp of a future date when the pre-signed URL request will expire and cannot be used any more.
_ | false | | Can be used as a cache buster to force a users browser to load the latest result. A good value for this would be the current unix timestamp. If using pre-signed URLs, do *not* include this parameter when generating the hash.
<aside class="success">
You can use either Basic HTTP Authentication or Pre-signed URLs to make Category API requests.
</aside>

## Category Lookup

> Example JSON response:

> <a target="_blank" href="https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3">https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3</a>

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "categories": [
                "informationtech",
                "business"
            ],
            "url": "webshrinker.com"
        }
    ]
}
```

```shell
curl -u "access-key:secret-key" "https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t"

or

curl "https://api.webshrinker.com/categories/v2/d2Vic2hyaW5rZXIuY29t?key=Xtf5w8wFGjX1OCHcmVok&hash=66eb681f798372718a5e272a86d204b3"
```

```php
<?php

function webshrinker_categories_v2($access_key, $secret_key, $url="", $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("categories/v2/%s?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$url = "https://www.webshrinker.com";
$request = webshrinker_categories_v2($access_key, $secret_key, $url);

// Initialize cURL and use pre-signed URL authentication
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $request);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

$response = curl_exec($ch);
$status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

print_r(json_decode($response));

switch($status_code) {
	case 200:
		// Do something with the JSON response
		break;
	case 202:
		// The response may have categories but the system is calculating them again,
		// check back soon
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

from urllib import urlencode
from base64 import b64encode
import hashlib
import requests

def webshrinker_categories_v2(access_key, secret_key, url="", params={}):
    params['key'] = access_key

    request = "categories/v2/%s?%s" % (b64encode(url), urlencode(params, True))
    signed_request = hashlib.md5("%s:%s" % (secret_key, request)).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

url = "https://www.webshrinker.com"

api_url = webshrinker_categories_v2(access_key, secret_key, url)
response = requests.get(api_url)

status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print data
elif status_code == 202:
    # The website is being visited and the categories will be updated shortly
    print data
elif status_code == 400:
    # Bad or malformed HTTP request
    print "Bad or malformed HTTP request"
    print data
elif status_code == 401:
    # Unauthorized
    print "Unauthorized - check your access and secret key permissions"
    print data
elif status_code == 402:
    # Request limit reached
    print "Account request limit reached"
    print data
else:
    # General error occurred
    print "A general error occurred, try the request again"
```

This endpoint returns a JSON response with the categories associated with the given URL, hostname, or IP address.

If a non-HTTP 200 response is returned then an "error" attribute will be returned as part of the response. The "error" array contains a human readable message to help with debugging the error condition.

### HTTP Request

`GET https://api.webshrinker.com/categories/v2/<url>`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
url | true | | The URL is part of the request and not an additional parameter. It should be Base64-encoded for maximum compatibility. It can be a full URL, a domain name, or an IP address.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
key | true (if using Pre-signed URLs) | | Your account access key to use for the request.
expires | false | 0 | A unix timestamp of a future date when the pre-signed URL request will expire and cannot be used any more.
_ | false | | Can be used as a cache buster to force a users browser to load the latest result. A good value for this would be the current unix timestamp. If using pre-signed URLs, do *not* include this parameter when generating the hash.
<aside class="success">
You can use either Basic HTTP Authentication or Pre-signed URLs to make Category API requests.
</aside>

<aside class="notice">
Keep in mind: URLs can be classified into one or more categories. They can also be classified as "uncategorized" if there isn't enough information to determine a proper category.
</aside>

### Category State

An HTTP 200 response indicates that the JSON contains the most current, up-to-date categories for the given URL.

An HTTP 202 response is given when the categories are being calculated for the given URL. If you check back again soon the categories for the URL will be updated.

# Errors

> Example bad request response:

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
    "error": {
        "message": "Invalid signed hash or no API key/secret given"
    }
}
```

Status Code | Meaning
----------- | -------
200 | OK -- The request was successful, the most recent categories are returned
202 | Accepted -- Your request was successful but is still being processed on the server, check back soon
400 | Bad Request -- One or more parameters in the request are invalid
401 | Unauthorized -- Your API key/secret key is wrong or the key doesn't have permission
402 | Payment Required -- Your account balance is used up, purchase additional requests in the account dashboard
412 | Precondition Failed -- Unable to satisfy the category request because there is not enough information available to classify the given URL
429 | Too Many Requests -- Too many requests in a short time
500 | Internal Server Error -- There was an issue processing the request, try again

