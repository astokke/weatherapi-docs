---
title: General forecast JSON format
date: 2020-05-03
author: Håvard Futseter
layout: page
---
This json format is meant for encoding meteorological and oceanographic forecast timeseries data for a specific geographical point on earth.

This documentation has two parts. The first part describes the structure of the json format. The second part describes how this format is used in our services to represent forecast data.

# Format
The json format has three main parts:
 - Geographical description
 - Forecast metadata
 - Forecast timeseries.
 
Here is an excerpt of a forecast response:
```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      10,
      60.1,
      267
    ]
  },
  "properties": {
    "meta": {
      "updated_at": "2020-06-10T13:04:26Z",
      "units": {
        "air_pressure_at_sea_level": "hPa",
        "air_temperature": "celsius",
        "air_temperature_max": "celsius",
        "air_temperature_min": "celsius",
        "cloud_area_fraction": "%",
        "cloud_area_fraction_high": "%",
        "cloud_area_fraction_low": "%",
        "cloud_area_fraction_medium": "%",
        "dew_point_temperature": "celsius",
        "fog_area_fraction": "%",
        "precipitation_amount": "mm",
        "precipitation_amount_max": "mm",
        "precipitation_amount_min": "mm",
        "probability_of_precipitation": "%",
        "probability_of_thunder": "%",
        "relative_humidity": "%",
        "ultraviolet_index_clear_sky": "1",
        "wind_from_direction": "degrees",
        "wind_speed": "m/s",
        "wind_speed_of_gust": "m/s"
      }
    },
    "timeseries": [
      {
        "time": "2020-06-10T13:00:00Z",
        "data": {
          "instant": {
            "details": {
              "air_pressure_at_sea_level": 1020.5,
              "air_temperature": 20.7,
              "cloud_area_fraction": 58.0,
              "cloud_area_fraction_high": 47.7,
              "cloud_area_fraction_low": 17.7,
              "cloud_area_fraction_medium": 1.7,
              "dew_point_temperature": 9.5,
              "fog_area_fraction": 0.0,
              "relative_humidity": 48.6,
              "ultraviolet_index_clear_sky": 4.7,
              "wind_from_direction": 151.8,
              "wind_speed": 2.5,
              "wind_speed_of_gust": 6.6
            }
          },
          "next_12_hours": {
            "summary": {
              "symbol_code": "partlycloudy_day"
            },
            "details": {
              "probability_of_precipitation": 2.2
            }
          },
          "next_1_hours": {
            "summary": {
              "symbol_code": "partlycloudy_day"
            },
            "details": {
              "precipitation_amount": 0.0,
              "precipitation_amount_max": 0.0,
              "precipitation_amount_min": 0.0,
              "probability_of_precipitation": 0.0,
              "probability_of_thunder": 0.0
            }
          },
          "next_6_hours": {
            "summary": {
              "symbol_code": "partlycloudy_day"
            },
            "details": {
              "air_temperature_max": 20.7,
              "air_temperature_min": 18.4,
              "precipitation_amount": 0.0,
              "precipitation_amount_max": 0.0,
              "precipitation_amount_min": 0.0,
              "probability_of_precipitation": 1.1
            }
          }
        }
      },
    [..]
```
You can get a complete forecast respons here: https://api.met.no/weatherapi/locationforecast/2.0/complete?lat=60.10&lon=10

## Geographical description

```json
"type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      10,
      60.1,
      267
    ]
  },
```

We use the geojson format to structure our json document. If you are unfamiliar with that format, you can read more about it [here](https://geojson.org/).

We only use a small subset of the geojson specification. Our responses are of type `Feature` only, and we only use the geometry type `Point`.

The rest of json document are defined under the geojson attribute called `properties`.

## Forecast metadata
```json
"meta": {
      "updated_at": "2020-06-10T13:04:26Z",
      "units": {
        "air_pressure_at_sea_level": "hPa",
        "air_temperature": "celsius",
         "air_temperature_max": "celsius",
        "air_temperature_min": "celsius",
        "cloud_area_fraction": "%",
        "cloud_area_fraction_high": "%",
        "cloud_area_fraction_low": "%",
        "cloud_area_fraction_medium": "%",
        "dew_point_temperature": "celsius",
        "fog_area_fraction": "%",
        "precipitation_amount": "mm",
        "precipitation_amount_max": "mm",
        "precipitation_amount_min": "mm",
        "probability_of_precipitation": "%",
        "probability_of_thunder": "%",
        "relative_humidity": "%",
        "ultraviolet_index_clear_sky": "1",
        "wind_from_direction": "degrees",
        "wind_speed": "m/s",
        "wind_speed_of_gust": "m/s"
      }
    },
```

Currently we have only two pieces of metadata in our forecast data. 

The first bit is `updated_at`. This specifies the most recent time when we updated the forecast data.

The second piece of metadata is `units`. The unit of all forecast parameters listed in the forecast data are listed in this block. The units are listed in each forecast document, but the unit for e.g `air_temperature` will be same for all locations. We will notify you about any change in unit values for a parameter. 


## Forecast timeseries
The forecast timeseries is structured as an array of forecast objects. The array is always sorted with increasing time.

Each forecast object contains a `time` attribute and a number of forecast parameters for that time. We have two main types of forecast parameters:
- parameters for a time instant.
- parameters for a time period.


### Parameters for a time instant 
These parameters are found under the `instant` object. These parameters, e.g `air_temperature` has a value that describes the state at that exact time instant.

### Parameters for a time period
These parameters are found under a number of objects: `next_1_hours`, `next_6_hours`, `next_12_hours`.  These parameters, e.g `precipitation_amount` describe a period of time. E.g `precipitation_amount` under the object `next_1_hours` describe the amount of forecasted precipitation for the period `time` + 1 hour.

The parameters under the object `summary` describes the weather situation based on many of the other parameters. E.g `symbol_code` will describe the weather situation for period of time, and includes information about clouds, precipitation and more.

# Services
You get short and medium meteorological forecast data from https://api.met.no/weatherapi/locationforecast/2.0. This service has three forecast data endpoints:
 - /compact
 - /complete 
 - /classic 

## /locationforecast/2.0/complete
This service endpoint will return a json document will all available forecast parameters. As we make new forecast parameters available, they will be added to the responses for this service endpoint.

Also, some geographical areas will have more forecast parameters available than others.

## /locationforecast/2.0/compact
This service endpoint will have only a core set of forecast parameters. All forecast parameters in this endpoint will be available for every location. We will add new parameters to this endpoint as well, but much more rarely, and the response will not increase much.

## /locationforecast/2.0/classic
This service endpoint exists only for backwards compatibility purposes. The parameters in this endpoint and the xml-format provided are identical with `/locationforecast/1.9`. 

This endpoint will be end-of-lifed in the future, and clients should migrate to one of the other two endpoints.


Stikkord:

- utvidelse for percentiler