---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://api.geofenceapi.org/'>GeofenceAPI.org</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the GeofenceAPI docs! You can use our API to create, modify, and query Geofences.

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

## Overview

A Geofence object is comprised of one or many <a href="http://geojson.org/" target="_blank">GeoJSON</a> polygons.

The workflow for creating a geofence is as follows:

1. Create the initial geofence
2. Add GeoJSON polygons
3. Compile the geofence
4. Send queries to GeofenceAPI

## Create a Geofence

```shell
curl 
  -XPOST 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
  -d '{"name": "My new geofence"}'
  'https://api.geofenceapi.org/v1/geofence'
```

> The above command returns JSON structured like this:

```
{
  "id": "91047298638649434",
  "name": "My new geofence"
}
```

Callers shoud HTTP POST a JSON document containing a name key.

## Update a Geofence

```shell
curl 
  -XPUT 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
  -d '{
  "name": "My updated name"
}' 
 'https://api.geofenceapi.org/v1/geofence/91047298638649434'
```

This endpoint updates an existing Geofence.

The format used is:

`PUT https://api.geofenceapi.org/v1/geofence/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the geofence to update

### JSON Parameters

Parameter | Description
--------- | -----------
name | The updated name

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

## Delete a Geofence

```shell
curl 
  -XDELETE 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
 'https://api.geofenceapi.org/v1/geofence/91290730848360488'
```

This endpoint deletes an existing geofence and all of it's GeoJSON items.

The format used is:

`DELETE https://api.geofenceapi.org/v1/geofence/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the geofence to delete

## Compile a Geofence

```shell
curl 
  -XPOST 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
  -d '{"geofence_id": "91047298638649434"}'
  'https://api.geofenceapi.org/v1/geofence/recompilerequest'
```

The "compile" action examines each of the GeoJSON items attached to a Geofence, computes the areas of intersection, applies the merge strategy for the GeoJSON properties, and saves the results into a highly efficient format used for runtime queries. You must invoke the compilation command in order for geofence changes to be applied.

### HTTP Request

`POST https://api.geofenceapi.org/v1/geofence/recompilerequest`

### JSON Parameters

Parameter | Description
--------- | -----------
geofence_id | The ID of the geofence to compile

# GeoJSON Items

## Add a GeoJSON Item

```shell
curl 
  -XPOST 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
  -d '{
  "geofence_id": "91047298638649434",
  "name": "My test GeoJSON item",
  "geojson": {
    "type": "FeatureCollection",
    "features": [
      {
        "geometry": {
          "type": "Polygon",
          "coordinates": [
            [
              [
                -122.4540826678276,
                37.77195292544332
              ],
              [
                -122.45389759540556,
                37.77103912851925
              ],
              [
                -122.44077891111374,
                37.77274374289986
              ],
              [
                -122.44095057249069,
                37.773608756061776
              ],
              [
                -122.4540826678276,
                37.77195292544332
              ]
            ]
          ]
        },
        "type": "Feature",
        "properties": {
          "foo": "bar"
        }
      }
    ]
  }
}' 
 'https://api.geofenceapi.org/v1/geojsonitem'
```

This endpoint adds a new GeoJSON document on to the Geofence.

When adding a new GeoJSON document, you specify the geofence ID, the level that the polygon should be bound to, and a name for reference.

*level* refers to the z-index level that the polygon is attached to. This becomes useful when more than one polygon's area overlaps the same place. GeofenceAPI will merge the properties together of intersection polygons according to their level with properties on higher levels superceeding those on lower levels.

## Update a GeoJSON Item

```shell
curl 
  -XPUT 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
  -d '{
  "name": "The GeoJSON item",
  "level": 1,
  "geojson": {
    "type": "FeatureCollection",
    "features": [
      {
        "geometry": {
          "type": "Polygon",
          "coordinates": [
            [
              [
                -122.4540826678276,
                37.77195292544332
              ],
              [
                -122.45389759540556,
                37.77103912851925
              ],
              [
                -122.44077891111374,
                37.77274374289986
              ],
              [
                -122.44095057249069,
                37.773608756061776
              ],
              [
                -122.4540826678276,
                37.77195292544332
              ]
            ]
          ]
        },
        "type": "Feature",
        "properties": {
          "foo": "bar"
        }
      }
    ]
  }
}' 
 'https://api.geofenceapi.org/v1/geojsonitem/91290730848360488'
```

This endpoint updates an existing GeoJSON item.

The format used is:

`PUT https://api.geofenceapi.org/v1/geojsonitem/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the geojsonitem to update

### JSON Parameters

Parameter | Description
--------- | -----------
name | The updated name
level | The updated level integer
geojson | The updated geojson

## Delete a GeoJSON Item

```shell
curl 
  -XDELETE 
  -H "Content-type: application/json"
  -H "AuthToken: YOURPRIVATEKEYHERE"
 'https://api.geofenceapi.org/v1/geojsonitem/91290730848360488'
```

This endpoint deletes an existing GeoJSON item.

The format used is:

`DELETE https://api.geofenceapi.org/v1/geojsonitem/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the geojsonitem to update

# Queries

> To create authorized curl requests, specify the AuthToken header:

```shell
# With shell, you can just pass the correct header with each request
curl 
-H "AuthToken: YOURPRIVATEKEYHERE" 
-H "Content-type: application/json" 
 "https://api.geofenceapi.org/v1/query/"\
 "91047298638649434?lat=41.37955647644566"\
 "&lng=-69.73571777347887"
```

> Make sure to replace `YOURPRIVATEKEYHERE` with your private API key and to replace the geofence id with one that's your own. The server will respond with soemthing like this:

```json
{
  "payload": {
    "localteam": "Red Sox"
  }
}
```

When the query point intersects one of the polygons, a 200 HTTP response code is returned with a JSON document containing a payload key. The payload value is the result of merging all of the GeoJSON properties together that intersected the query point.

<aside class="notice">
When the lat/lng query point does not intersect any polygon, a 404 HTTP response code is returned.
</aside>
