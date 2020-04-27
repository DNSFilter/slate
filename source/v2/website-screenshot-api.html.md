---
title: Website Screenshots API Reference

language_tabs:
  - shell
  - php
  - python

toc_footers:
  - <a href='https://dashboard.webshrinker.com/register'>Register for a free account</a>
  - <a href='https://www.webshrinker.com/'>Main Webshrinker Site</a>

includes:
  - errors

search: true
---

# Screenshot API Introduction

> Example of what a website screenshot can look like:

><img src="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4" />

><a target="_blank" href="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4">https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4</a>

> Example of what a pixelated screenshot can look like (used when blurring sites that contain "adult" content):

><img src="/images/sample_pixelated_image.png" />

The Webshrinker screenshots API gives developers the ability to capture an image of what a particular website looks like in a browser window. You can then include these images in your own web pages or do some other processing on them in your preferred programming language. This technique has been used on many popular sites including the major search engines. 

While screenshot thumbnails are commonly used on web pages they can also easily be integrated into other applications and frameworks. For instance, say you have a PHP application that needs images of web sites so they can be included in a PDF report, or an application which tracks visual changes on web sites. The possibilities are endless. That’s why we try to make integration with Webshrinker as easy as possible by offering SDKs and allowing developers access to the underlying API via simple HTTP requests.

It is also possible to automatically pixelate screenshots for websites that are categorized as an "adult" site, reducing the chances that you would be displaying questionable content to your users.

# Authentication

To make API requests you need to have an access key and secret key. If you don't have an access key yet, you can register for a free account on our [account dashboard](https://dashboard.webshrinker.com/register).

There are two methods of authentication that can be used to make API requests: Basic HTTP Authentication and Pre-signed URLs.

## Basic HTTP Authentication

> Example HTTP request:

```http
GET /thumbnails/v2/aHR0cHM6Ly93d3cuZmFjZWJvb2suY29t?size=xlarge HTTP/1.1
Authorization: Basic Yzk1NDJkMDFkNjlmOjE3OTAyYzJjOWIzYQ==
Host: api.webshrinker.com
```

Basic authentication requires that you send your API access and secret key with each request to the service over HTTPS. Most programming frameworks and SDKs support sending Basic HTTP Authentication “out of the box” with a simple function call.

The access key and secret key are used as the “username” and “password” for Basic Authentication.

If you are crafting the "Authorization" header yourself it is the word "Basic" followed by the base64-encoded string "your-access-key:your-secret-key". Additional information about this HTTP header can be found on [Basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) on Wikipedia.

## Pre-signed URLs

> Example pre-signed screenshot URL:

> <img src="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4" />

> <a target="_blank" href="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4">https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4</a>

Generating a pre-signed URL allows you to make requests without revealing your secret key. It’s perfect for situations where you need to embed a request in an application or in a webpage but don’t want users to know your secret key, preventing a third party from making unauthorized requests against your account.

The URL used to make the API request is signed using your access and secret key but the URL itself doesn’t contain the secret key. Instead it contains an extra parameter called "hash".

The "hash" is the MD5 hash of your secret key, a colon (":"), and the request URL with query parameters. 

```shell
WS_ACCESS_KEY="your access key"
WS_SECRET_KEY="your secret key"

URL=($(echo -n "http://www.example.com" | base64))

REQUEST="thumbnails/v2/$URL?key=$WS_ACCESS_KEY&size=xlarge"
HASH=($(echo -n "$WS_SECRET_KEY:$REQUEST" | (md5sum || md5)))

echo "https://api.webshrinker.com/$REQUEST&hash=$HASH"
```

```php
<?php

function webshrinker_v2($access_key, $secret_key, $url, $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("thumbnails/v2/%s?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$url = "http://www.example.com/";
$signedUrl = webshrinker_v2($access_key, $secret_key, $url, [
	'size' => 'xlarge'
]);

echo "$signedUrl\n";

?>
```

```python
try:
    from urllib import urlencode
except ImportError:
    from urllib.parse import urlencode

from base64 import urlsafe_b64encode
import hashlib

def webshrinker_v2(access_key, secret_key, url, params):
    params['key'] = access_key

    request = "thumbnails/v2/%s?%s" % (urlsafe_b64encode(url).decode('utf-8'), urlencode(params, True))
    request_to_sign = "{}:{}".format(secret_key, request).encode('utf-8')
    signed_request = hashlib.md5(request_to_sign).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

params = {
    'size': 'xlarge'
}

url = b"http://www.example.com"

print(webshrinker_v2(access_key, secret_key, url, params))
```

# Making Requests

## Image Requests

```shell
curl -v -u "access-key:secret-key" "https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==?size=xlarge" -o screenshot.png
```

```php
<?php

function webshrinker_v2($access_key, $secret_key, $url, $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("thumbnails/v2/%s?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$url = "http://www.example.com";
$request = webshrinker_v2($access_key, $secret_key, $url, [
	'size' => 'xlarge'
]);

// Initialize cURL and use pre-signed URL authentication
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $request);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

$image_data = curl_exec($ch);
$status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

switch($status_code) {
	case 200:
		// $image_data contains the screenshot of the site
		// Do something with the image
		echo "The screenshot was saved to: screenshot.png\n";
		file_put_contents('screenshot.png', $image_data);
		break;
	case 202:
		// The website is being visited and the screenshot created
		// $image_data contains the placeholder loading image
		echo "The screenshot is being created, the placeholder image was saved to: screenshot.png\n";
		file_put_contents('screenshot.png', $image_data);
		break;
	case 400:
		// Bad or malformed HTTP request
		echo "Bad or malformed HTTP request\n";
		break;
	case 401:
		// Unauthorized
		echo "Unauthorized - check your access and secret key permissions\n";
		break;
	case 402:
		// Request limit reached
		echo "Account request limit reached\n";
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

try:
    from urllib import urlencode
except ImportError:
    from urllib.parse import urlencode

from base64 import urlsafe_b64encode
import hashlib
import requests

def webshrinker_v2(access_key, secret_key, url, params):
    params['key'] = access_key

    request = "thumbnails/v2/%s?%s" % (urlsafe_b64encode(url).decode('utf-8'), urlencode(params, True))
    request_to_sign = "{}:{}".format(secret_key, request).encode('utf-8')
    signed_request = hashlib.md5(request_to_sign).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

params = { 
    'size': 'xlarge' 
}

url = b"https://www.webshrinker.com"

# Make a screenshot API call using pre-signed URL authentication
api_url = webshrinker_v2(access_key, secret_key, url, params)
response = requests.get(api_url)

image_data = response.content
status_code = response.status_code

if status_code == 200:
    # image_data contains the screenshot of the site
    # Do something with the image
    print("Screenshot image retrieved and saved to 'screenshot.png'")

    f = open("screenshot.png", "wb")
    f.write(image_data)
    f.close()
elif status_code == 202:
    # The website is being visited and the screenshot created
    # image_data contains the placeholder loading image
    print("The screenshot is being created, the placeholder image was saved to 'screenshot.png'")

    f = open("screenshot.png", "wb")
    f.write(image_data)
    f.close()
elif status_code == 400:
    # Bad or malformed HTTP request
    print("Bad or malformed HTTP request")
elif status_code == 401:
    # Unauthorized
    print("Unauthorized - check your access and secret key permissions")
elif status_code == 402:
    # Request limit reached
    print("Account request limit reached")
else:
    # General error occurred
    print("A general error occurred, try the request again")

```

This endpoint returns a PNG image in the size requested for the specified URL.

If you need to know the status of a screenshot use the "[Information Request](#information-requests)" JSON endpoint.

### HTTP Request

`GET https://api.webshrinker.com/thumbnails/v2/<url>`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
url | true | | The URL is part of the request and not an additional parameter. It should be Base64-encoded for maximum compatibility.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
key | true (if using Pre-signed URLs) | | Your account access key to use for the request.
size | true | | The size of the image to be returned. Valid values: micro, tiny, verysmall, small, large, xlarge, 2xlarge, 3xlarge, 4xlarge, or a custom size like 320x240. *Use size or width, not both.*
delay | false | 0 | An extra amount of seconds to wait before taking the screenshot.
expires | false | 0 | A unix timestamp of a future date when the pre-signed URL request will expire and cannot be used any more.
fullpage | false | false | Returns a screenshot of the entire webpage and not just what is visible in the viewport.
hideadult | false | 0 | Automatically detect and return a pixelated/blurred image if the URL or domain name is categorized as "adult".
refresh | false | false | Returns the most recent screenshot if it's available. It will also request that the URL be visited again and a fresh screenshot be captured.
pixelate | false | 2/4/6/10/16 | When using the 'hideadult' option, apply this amount of pixelation to the screenshot image.
viewport | false | 1280x1024 | The width and height that the browser window will use for the screenshot capture. Think of this as the users screen size.
width | false | | Set the width for the returned image, maintaining aspect ratio. Max value is 2048. *Use size or width, not both.*
_ | false | | Can be used as a cache buster to force a users browser to load the latest screenshot image. A good value for this would be the current unix timestamp. If using pre-signed URLs, do *not* include this parameter when generating the hash.
<aside class="success">
You can use either Basic HTTP Authentication or Pre-signed URLs to make image requests.
</aside>

## Information Requests

> Example JSON response:

> <a target="_blank" href="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==/info?key=TvQu6ARhl2Zs7BVV1plU&size=xlarge&hash=186514ed46eb14b71fa9b4edebe6bec3">https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==/info?key=TvQu6ARhl2Zs7BVV1plU&size=xlarge&hash=186514ed46eb14b71fa9b4edebe6bec3</a>

```json
{
    "data": [
        {
            "image": "https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==?size=xlarge&key=TvQu6ARhl2Zs7BVV1plU&hash=97462c219208614dec16cf9098433f6f",
            "state": "READY",
            "updated": "Mon, 30 May 2016 23:13:06 +0000",
            "url": "https://www.webshrinker.com/"
        }
    ]
}
```

```shell
curl -v -u "access-key:secret-key" "https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==/info"
```

```php
<?php

function webshrinker_info_v2($access_key, $secret_key, $url, $options=array()) {
	$options['key'] = $access_key;

	$parameters = http_build_query($options);

	$request = sprintf("thumbnails/v2/%s/info?%s", base64_encode($url), $parameters);
	$hash = md5(sprintf("%s:%s", $secret_key, $request));

	return "https://api.webshrinker.com/{$request}&hash={$hash}";
}

$access_key = "your access key";
$secret_key = "your secret key";

$url = "http://www.example.com";
$request = webshrinker_info_v2($access_key, $secret_key, $url, [
	'size' => 'xlarge'
]);

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
		echo "Bad or malformed HTTP request\n";
		break;
	case 401:
		// Unauthorized
		echo "Unauthorized - check your access and secret key permissions\n";
		break;
	case 402:
		// Request limit reached
		echo "Account request limit reached\n";
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

try:
    from urllib import urlencode
except ImportError:
    from urllib.parse import urlencode

from base64 import urlsafe_b64encode
import hashlib
import requests

def webshrinker_info_v2(access_key, secret_key, url, params):
    params['key'] = access_key

    request = "thumbnails/v2/%s/info?%s" % (urlsafe_b64encode(url).decode('utf-8'), urlencode(params, True))
    request_to_sign = "{}:{}".format(secret_key, request).encode('utf-8')
    signed_request = hashlib.md5(request_to_sign).hexdigest()

    return "https://api.webshrinker.com/%s&hash=%s" % (request, signed_request)

access_key = "your access key"
secret_key = "your secret key"

params = { 
    'size': 'xlarge' 
}

url = b"http://www.example.com"

# Make a screenshot API call using pre-signed URL authentication
api_url = webshrinker_info_v2(access_key, secret_key, url, params)
response = requests.get(api_url)

status_code = response.status_code
data = response.json()

if status_code == 200:
    # Do something with the JSON response
    print(data)
elif status_code == 202:
    # The website is being visited and the screenshot created
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

This endpoint returns a JSON response for the requested URL. The response includes the last updated/visited timestamp and the current screenshot status.

### HTTP Request

`GET https://api.webshrinker.com/thumbnails/v2/<url>/info`

### URL

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
url | true | | The URL is part of the request and not an additional parameter. It should be Base64-encoded for maximum compatibility.

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
key | true (if using Pre-signed URLs) | | Your account access key to use for the request.

### Screenshot Parameters

If the "size" or "width" parameter is included in the information request then a pre-signed screenshot URL will be present in the JSON response. You can pass other screenshot parameters to be included with the pre-signed image URL.

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
delay | false | 0 | An extra amount of seconds to wait before taking the screenshot.
expires | false | 0 | A unix timestamp of a future date when the pre-signed URL request will expire and cannot be used any more.
fullpage | false | false | Returns a screenshot of the entire webpage and not just what is visible in the viewport.
hideadult | false | 0 | Automatically detect and return a pixelated/blurred image if the URL or domain name is categorized as "adult".
pixelate | false | 2/4/6/10/16 | When using the 'hideadult' option, apply this amount of pixelation to the screenshot image.
refresh | false | false | Returns the most recent screenshot if it's available. It will also request that the URL be visited again and a fresh screenshot be captured.
size | false | | The size of the image to be returned. Valid values: micro, tiny, verysmall, small, large, xlarge, 2xlarge, 3xlarge, 4xlarge, or a custom size like 320x240. *Use size or width, not both.*
viewport | false | 1280x1024 | The width and height that the browser window will use for the screenshot capture. Think of this as the users screen size.
width | false | | Set the width for the returned image, maintaining aspect ratio. Max value is 2048. *Use size or width, not both.*
_ | false | | Can be used as a cache buster to force a users browser to load the latest screenshot image. A good value for this would be the current unix timestamp. If using pre-signed URLs, do *not* include this parameter when generating the hash.
<aside class="success">
You can use either Basic HTTP Authentication or Pre-signed URLs to make information/JSON requests.
</aside>

### Screenshot State

> Example JSON response for a screenshot that is being refreshed / revisited:

```json
{
    "data": [
        {
            "image": "https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29tLw==?size=xlarge&key=TvQu6ARhl2Zs7BVV1plU&hash=97462c219208614dec16cf9098433f6f",
            "state": "REFRESH",
            "updated": "Mon, 30 May 2016 18:03:26 +0000",
            "url": "https://www.webshrinker.com/"
        }
    ]
}
```

The "state" field in the JSON response represents the current status of the screenshot image.

State | Description
----- | -----------
READY | the URL was visited and the screenshot image is ready
REFRESH | an older screenshot image is available but we're revisiting the URL and generating a new image
PROCESSING | the URL is being visited now and the screenshot will be created
ERROR | an error happened while visiting the destination URL and we weren't able to generate a screenshot

# Screenshot Sizes

You can use a short name for the "size" parameter for screenshots or your own custom size. Custom sizes are given in "widthxheight" format, such as: 320x240, 800x600, or 1024x768.

These short names can be used for the "size" parameter instead of specifying the full dimensions:

Short Name | Width x Height
---------- | --------------
micro | 75 x 56
tiny | 90 x 68
verysmall | 100 x 75
small | 120 x 90
large | 200 x 150
xlarge | 320 x 240
2xlarge | 550 x 412
3xlarge | 1024 x 768
4xlarge | 1280 x 1024
