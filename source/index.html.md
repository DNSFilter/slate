---
title: API Reference

toc_footers:
  - <a href='https://app.webshrinker.com/signup'>Register for a free account</a>
  - <a href='https://www.webshrinker.com/'>Main Webshrinker Site</a>

search: true
---

# Introduction

Webshrinker is a SaaS (Software as a Service) provider. We offer APIs that can be used in a variety of programming languages and, in some cases, directly within HTML code. Currently we have three public facing APIs: [Website Category Lookup API](https://www.webshrinker.com/apis/#domain-category), [Website Domain API](https://www.webshrinker.com/apis/#domain-data), and [Website Screenshot API](https://www.webshrinker.com/apis/#screenshot).

# Website Category API

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "data": [
        {
            "categories": [
                {
                    "confident": true,
                    "id": "IAB19",
                    "label": "Technology & Computing",
                    "parent": "IAB19",
                    "score": "0.855809166500086094"
                },
                {
                    "confident": true,
                    "id": "IAB19-18",
                    "label": "Internet Technology",
                    "parent": "IAB19",
                    "score": "0.824063117153139624"
                }
            ],
            "url": "webshrinker.com"
        }
    ]
}
```

Webshrinker utilizes cloud computing and machine learning to classify websites into a set of categories. The resulting URL to category mappings are made available as an online API and also as a downloadable list so you can integrate and use it with other software and services.

Perhaps you’re building an Internet website filter and need to block certain categories such as adult and social networking sites. Maybe you are running a hosting company and want to enforce an Acceptable Use Policy to prevent customers from running a file sharing or classified ads network. These would be two possible examples of how the website category lookup API can be used.

The current API version is v3.

[View the Category v3 API documentation](/v3/website-category-api.html)

# Website Domain API

<aside class="warning">
<b>The Domain API will be discontinued as of Dec 31, 2020. It is not recommended to use this API for any new integrations as it will no longer be available after this time.</b>
</aside>

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
                "socialnetworking"
            ],
            "host": "twitter.com",
            "related": [
                "2013.twitter.com",
                "yearinreview.twitter.com",
                "apps.twitter.com",
                "analytics.twitter.com",
                "search.twitter.com",
                "bloombergtechnology.twitter.com",
                "goldenglobes.twitter.com",
                "assets3.twitter.com",
                "blog.uk.twitter.com",
                "jobs.twitter.com",
                "assets4.twitter.com",
                "marketing.twitter.com",
                "cards.twitter.com",
                "media.twitter.com",
                "translate.twitter.com",
                "assets2.twitter.com",
                "pic.twitter.com",
                "about.twitter.com",
                "music.twitter.com",
                "dev.twitter.com"
            ],
            "addresses": {
                "ipv4": {
                    "104.244.42.129": [
                        "2017-08-11T18:56:06Z"
                    ],
                    "104.244.42.65": [
                        "2017-08-09T11:26:34Z"
                    ],
                    "104.244.42.1": [
                        "2017-07-10T15:07:23Z"
                    ],
                    "199.16.156.102": [
                        "2016-10-20T21:18:01Z"
                    ],
                    "199.16.156.230": [
                        "2016-08-31T00:00:00Z"
                    ]
                }
            }
        }
    ],
    "paging": {
        "cursors": {},
        "next": "",
        "count": 25
    }
}
```

The Webshrinker Domain API gives you the ability to search through and retrieve data that we accumulate from analyzing billions of URLs each month.

Request information on a specific domain name or IP address: Returns the primary language used for the site, its main categories, the server IP addresses used to host the site, as well as known sub-domain names.

Retrieve domain backlinks: Returns a list of external websites that link to the requested domain name, including link attributes such as 'rel', 'text', and 'alt'.

Retrieve domain outbound links: Returns a list of websites that the requested domain name is linking to, including link attributes such as 'rel', 'text', and 'alt'.

The current API version is v3.

[View the Domain v3 API documentation](/v3/website-domain-api.html)

# Website Screenshot API

> <img src="https://api.webshrinker.com/thumbnails/v2/aHR0cHM6Ly93d3cud2Vic2hyaW5rZXIuY29t?key=TvQu6ARhl2Zs7BVV1plU&size=large&hash=d78783a0c6c2f0aced16a18d0b8d30f4" />

The Webshrinker screenshot/thumbnail API gives you the ability to capture an image of what a particular website looks like in a browser window. You can then include these images in your own web pages or do some other processing on them in your preferred programming language. This technique has been used on many popular sites including the major search engines. 

It is also possible to automatically pixelate screenshots for websites that are categorized as an "adult" site, reducing the chances that you would be displaying questionable content to your users.

An example of what a screenshot can look like can be seen next to this paragraph. The image being displayed is of the main Webshrinker web site generated by our servers using our own API.

The current API version is v2.

[View the Screenshot v2 API documentation](/v2/website-screenshot-api.html)
