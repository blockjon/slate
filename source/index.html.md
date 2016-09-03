---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href="https://api.geofenceapi.org/ui/docs">V1 original documentation</a>
  - <a href='https://api.geofenceapi.org/'>Visit the main site</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the GeofenceAPI docs! You can use our API to create, modify, and query Geofences.

Note: This documentation is still being written. [Slightly more complete documentation](https://api.geofenceapi.org/ui/docs) is available.

Since this is a plain REST/JSON API, you can use GeofenceAPI from any language. You can view code plain curl examples in the dark area to the right, and then use that to write an integration from your programming language.

# Content Type

> To create properly formed requests, always specify application/json via header:

```shell
# With shell, you can just pass the correct header with each request
curl -H "Content-type: application/json"
```

Callers should always specify the content type as "application/json" in all api calls.


# Authentication

> To create authorized curl requests, specify the AuthToken header:

```shell
# With shell, you can just pass the correct header with each request
curl -H "AuthToken: YOURKEYHERE"
```

> Make sure to replace `YOURKEYHERE` with your API key.

GeofenceAPI uses API keys to allow access to the API. There are two types of API Keys - public and private. In order to access your keys, log in to our [management console](https://api.geofenceapi.org/ui/login) and then click on "API Keys" from the drop down menu in the top right. 

*Unregistered Users:* If you do not yet have an account, simply click the "Create Geofence" button on the main page. An account is automatically generated.

Private keys allow you to create and modify geofences via the API. Public Keys are used to query geofences via the API.

GeofenceAPI expects for the API key to be included in all API requests to the server in a header that looks like the following:

`AuthToken: YOURKEYHERE`

<aside class="notice">
You must replace <code>YOURKEYHERE</code> with your API key.
</aside>

# Geofences

## Get a Specific Geofence

```shell
curl "https://api.geofenceapi.org/v1/geofence/91047298638649434"
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
```

> The above command returns JSON structured like this:

```json
{
  "id": "91047298638649434", 
  "name": "Default",
  "bounding_box": [
    -122.55523681640625, 
    37.640334898059486, 
    -122.33276367187499, 
    37.80761398306056
  ], 
  "compilation_failed": false, 
  "compilation_state": "fresh", 
  "created_at": "2016-09-02 01:30:01.176188-07:00", 
  "geojson_items": [
    {
      "id": "91290730848360488", 
      "level": 0, 
      "name": "My First Geofence",
      "created_at": "2016-09-02 17:14:39.663457-07:00", 
      "geojson": {
        "features": [
          {
            "geometry": {
              "coordinates": [
                  [
                    [
                        -122.55523681640625, 
                        37.640334898059486
                    ], 
                    [
                        -122.55523681640625, 
                        37.80761398306056
                    ], 
                    [
                        -122.33276367187499, 
                        37.80761398306056
                    ], 
                    [
                        -122.33276367187499, 
                        37.640334898059486
                    ], 
                    [
                        -122.55523681640625, 
                        37.640334898059486
                    ]
                ]
              ], 
              "type": "Polygon"
            }, 
            "properties": {
                "hello": "world"
            }, 
            "type": "Feature"
          }
        ], 
        "type": "FeatureCollection"
      }
    }
  ]
}

```

This endpoint retrieves a specific geofence.

### HTTP Request

`GET https://api.geofenceapi.org/v1/geofence/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the geofence to retrieve

