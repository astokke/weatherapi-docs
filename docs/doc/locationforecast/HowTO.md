---
title: Locationforecast HOWTO
date: 2020-05-11
author: Håvard Futseter
layout: page
state: draft
---

# Choose the endpoint for your needs
[Locationforecast/2.0](https://api.met.no/weatherapi/locationforecast/2.0) gives you a short -and medium weather forecast in a GeoJSON format.  

When starting to use this service, you need to figure out which endpoint to use for getting forecast data. The service provides three, described below.

If you are uncertain which to pick, start with `/weatherapi/locationforecast/2.0/compact`.

## /weatherapi/locationforecast/2.0/complete

This service endpoint will return a response with all available forecast
parameters. As we make new forecast parameters available, they will be added to
the responses for this service endpoint.

Also, some geographical areas will have more forecast parameters available than others.

## /weatherapi/locationforecast/2.0/compact

This service endpoint will return a response with only a core set of forecast
parameters. All forecast parameters in this endpoint will be available for every
location. We will add new parameters to this endpoint as well, but much more
rarely, and the response will not increase much.

## /weatherapi/locationforecast/2.0/classic

This service endpoint exists only for backwards compatibility purposes. The
parameters in this endpoint and the XML format provided are identical with
`/locationforecast/1.9`.

This endpoint will be end-of-lifed in the future **[Why?]**, and clients should
migrate to one of the other two endpoints.

# Start using the service
First, read about [the structure of the JSON format we use](../ForecastJSON.md).

Then, create your client in one of two ways:
- From an example response, e.g <https://api.met.no/weatherapi/locationforecast/2.0/compact?lat=60&lon=11>.
- From the api reference documentation: <https://api.met.no/weatherapi/locationforecast/2.0>

If you choose option 1, it would still be a good idea to later look at the api reference documentation to make sure you are not missing anything.

For more information about the meteorological content of the service, please go [here](datamodel.md)

# Important notes for client code
* Some parameters are not available for every geographic area, so please make sure your client can handle that.
* In rare cases, a parameter drops out due to an operational issue. Make sure to have error logic to handle missing parameters, for one single time but also for the case when the parameter is missing from the entire timeseries.

# Reminder
Also, please remember to follow our mailinglist about updates to the service.  Subscribe by sending an email to <api-users-request@lists.met.no> with the word "subscribe" in the subject line.

If you run into any difficulties, please let us know by sending an e-mail to <weatherapi-adm@met.no>

