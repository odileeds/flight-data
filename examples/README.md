# Data

We started collating flight emissions data for Leeds and Bradford airport to power our [Flight Emissions comparison tool](https://patricklake2.github.io/flight-emissions/). We realised that it would benefit others if the emissions data were openly available and if other people could contribute by doing the work for other airports. This project aims to allow different people/organisations to take responsibility for collating data for different airports and publish it to their own URLs but make it available to others so we can see the bigger picture of flight emissions.

In an attempt to keep things manageable, for both us and tools that load the data, we've split this up into three parts:

* [Flight data](#flight-data)
* [Airport metadata](#airport-metadata)
* [Primary index](#primary-index)

## Flight data

This contains the full details for each departure from an airport in a given day (*local time*). You should have one directory for your data, in it there should be a seperate JSON file for each day, where the filename is the date in ISO format (YYYY-MM-DD.json). By splitting the data into individual days, it keeps filesize more manageable rather than linearly growing with time e.g. Leeds and Bradford works out at under 5.5 kB per file.

The file should have the following format:

```javascript
{
	"from": {        // origin airport information
		"n": "Leeds Bradford",
		"ICAO": "EGNM",
		"IATA": "LBA",
		"geo": [-1.5,53.5],
		"cc": "GB",
		"continent": "EU"
	},
	"flights": [{
		"id":"KL1540",
		"time":"2019-12-11T06:10Z",
		"airline":"KLM",
		"aircraft":{"code":"E190","name":"EMBRAER ERJ190"},
		"to":{"n":"Amsterdam","ICAO":"EHAM","IATA":"AMS","geo":[4.7639,52.3086],"cc":"NL","continent":"EU" },
		"km":462.46,
		"emissions":{"f":10.2,"kg":4717.13}
	},{
		//another flight 
	}]
}
```

* `from` - the origin airport and should appear once per file. This helps if the file ends up on a different file system.
  * `n` - a free-form field for the displayed airport name
  * `ICAO` - the ICOA code (usually 4 characters)
  * `IATA` - the international airport code (usually 3 characters)
  * `geo` - a [`longitude`,`latitude`] array with the coordinates in degrees (compatible with GeoJSON)
  * `cc` - the two letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code for the origin
  * `continent` - a [two letter continent code](https://datahub.io/core/continent-codes) e.g. `AF`, `NA`, `OC`, `AN`, `AS`, `EU`, `SA`
* `flights` - an array of flights (in chronological order). Each flight contains:
  * `id` - the flight code
  * `time` - the [ISO8601 timestamp](https://en.wikipedia.org/wiki/ISO_8601) for the departure in local time *with* timezone included
  * `airline` - the name of the airline
  * `aircraft` - details of the aircraft broken down into:
    * `code` - the aircraft type code e.g. "E190"
    * `name` - the aircraft name e.g. "EMBRAER ERJ190"
  * `to` - the destination broken down into:
    * `n` - a free-form field for the displayed airport name
    * `ICAO` - the ICOA code
    * `IATA` - the international airport code
    * `geo` - a [`longitude`,`latitude`] array with the coordinates in degrees (compatible with GeoJSON)
    * `cc` - the two letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code for the origin
    * `continent` - a [two letter continent code](https://datahub.io/core/continent-codes) e.g. `AF`, `NA`, `OC`, `AN`, `AS`, `EU`, `SA`
  * `km` - the Great Circle distance in kilometres
  * `emissions` - the estimated CO2 emissions of the flight
    * `f` - the emissions factor
    * `kg` - the kilograms of CO2
    
### Getting data

You could perhaps scrape from an airport's website or use a paid API such as [Flight Aware](https://uk.flightaware.com/commercial/flightxml/).Flight Aware have a [`Scheduled`](https://uk.flightaware.com/commercial/flightxml/explorer/#op_Scheduled) end point that returns upcoming departures and gives aircraft type codes which then allows emissions factors to be found. That end point [costs $0.0079](https://uk.flightaware.com/commercial/flightxml/pricing_class.rvt) per request (which contains up to 15 flights). The response from Flight Aware's `Scheduled` end point looks like this:

```
{"ScheduledResult":{"next_offset":15,"scheduled":[{"ident":"RYR2446","aircrafttype":"B738","filed_departuretime":1579264200,"estimatedarrivaltime":1579274100,"origin":"EGNM","destination":"LEMG","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Malaga","destinationCity":"Malaga"},{"ident":"BEE736","aircrafttype":"DH8D","filed_departuretime":1579268700,"estimatedarrivaltime":1579271100,"origin":"EGNM","destination":"EGAC","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"George Best Belfast City","destinationCity":"Belfast, Northern Ireland"},{"ident":"STK3393","aircrafttype":"AT72","filed_departuretime":1579276200,"estimatedarrivaltime":1579279800,"origin":"EGNM","destination":"EIDW","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Dublin Int'l","destinationCity":"Dublin"},{"ident":"EIN3393","aircrafttype":"","filed_departuretime":1579276200,"estimatedarrivaltime":1579279800,"origin":"EGNM","destination":"EIDW","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Dublin Int'l","destinationCity":"Dublin"},{"ident":"RYR9079","aircrafttype":"B738","filed_departuretime":1579276200,"estimatedarrivaltime":1579284600,"origin":"EGNM","destination":"LEAL","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Alicante","destinationCity":"Alicante / Benidorm / Costa Blanca"},{"ident":"EZE7606","aircrafttype":"JS41","filed_departuretime":1579278000,"estimatedarrivaltime":1579281300,"origin":"EGNM","destination":"EGHI","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Southampton","destinationCity":"Southampton, England"},{"ident":"EXS205","aircrafttype":"B738","filed_departuretime":1579279200,"estimatedarrivaltime":1579282500,"origin":"EGNM","destination":"EHAM","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Amsterdam Schiphol","destinationCity":"Amsterdam"},{"ident":"BEE738","aircrafttype":"DH8D","filed_departuretime":1579280700,"estimatedarrivaltime":1579283100,"origin":"EGNM","destination":"EGAC","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"George Best Belfast City","destinationCity":"Belfast, Northern Ireland"},{"ident":"KLM1550","aircrafttype":"E170","filed_departuretime":1579281900,"estimatedarrivaltime":1579285500,"origin":"EGNM","destination":"EHAM","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Amsterdam Schiphol","destinationCity":"Amsterdam"},{"ident":"BAW1345","aircrafttype":"A319","filed_departuretime":1579284600,"estimatedarrivaltime":1579286700,"origin":"EGNM","destination":"EGLL","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"London Heathrow","destinationCity":"London, England"},{"ident":"RYR4107","aircrafttype":"B738","filed_departuretime":1579287900,"estimatedarrivaltime":1579294800,"origin":"EGNM","destination":"EPWR","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Copernicus Wrocław","destinationCity":"Wrocław"},{"ident":"EIN3395","aircrafttype":"","filed_departuretime":1579288200,"estimatedarrivaltime":1579291800,"origin":"EGNM","destination":"EIDW","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Dublin Int'l","destinationCity":"Dublin"},{"ident":"STK3395","aircrafttype":"AT72","filed_departuretime":1579288200,"estimatedarrivaltime":1579291800,"origin":"EGNM","destination":"EIDW","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Dublin Int'l","destinationCity":"Dublin"},{"ident":"RYR157","aircrafttype":"B738","filed_departuretime":1579290300,"estimatedarrivaltime":1579293000,"origin":"EGNM","destination":"EIDW","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"Dublin Int'l","destinationCity":"Dublin"},{"ident":"BEE740","aircrafttype":"DH8D","filed_departuretime":1579291800,"estimatedarrivaltime":1579294200,"origin":"EGNM","destination":"EGAC","originName":"Leeds Bradford Int'l","originCity":"West Yorkshire, England","destinationName":"George Best Belfast City","destinationCity":"Belfast, Northern Ireland"}]}}
```

This gives the flight number (`ident`), aircraft type code (`aircrafttype`), and destination ICAO code (`destination`) for each flight. It is limited to 15 results unless you also set `howMany=30` in the API call and make sure you have also (once) called the `SetMaximumResultSize` end point e.g. http://flightxml.flightaware.com/json/FlightXML2/SetMaximumResultSize?max_size=30.

You can convert ICAO codes to airport names, latitudes/longitudes, country code, and continent using this [big list of airports](https://github.com/datasets/airport-codes/blob/master/archive/data.csv) which will allow you to calculate the Great Circle distance between the origin and destination.

Opentraveldata have a [big maintained list of airlines](https://github.com/opentraveldata/opentraveldata/blob/master/data/IATA/archives/iata_airline_list_20180907.csv) in a separate repo. This is a `^` delimited file of the form:

```
2char_code^3char_code^num_code^name^type
...
BA^BAW^125^British Airways p.l.c.^I
FR^RYR^^Ryanair Ltd.^I
```

If you take the first three characters of the `ident` you can look up the airline name e.g. `RYR2446` gives us `RYR` which is `Ryanair`.

Alternatively, Opentraveldata have a [more current list of airlines](https://github.com/opentraveldata/opentraveldata/blob/master/opentraveldata/optd_airlines.csv) but it is not as comprehensive.

### Calculating CO2 emissions

You can estimate emissions using [Patrick Lake's methodology](https://github.com/patricklake2/flight-emissions/tree/master/leeds-bradford#data). Patrick extracted the aicraft emissions from [BEIS's Greenhouse gas reporting: conversion factors 2019](https://www.gov.uk/government/publications/greenhouse-gas-reporting-conversion-factors-2019) [methodology paper](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/829336/2019_Green-house-gas-reporting-methodology.pdf) and created a CSV. Each aircraft type code can have different emissions depending on the flight distance. In the paper they've used `Domestic`, `Short-haul`, and `Long-haul` which we are interpreting as distances of `up to 1000km`, `above 1000km and below 3700km` and `3700km and over`. But, because the Great Circle distance and aircraft type code are provided in the data, applications could recalculate the CO2 emissions differently if they need to or if methodologies improve. For example, the [Euro Control Small Emitters Tool](https://www.eurocontrol.int/publication/small-emitters-tool-set-2019) currently recommends adding 95km to the flight distance and it would be easy for a tool to add that.


## Airport metadata

Each individual airport has a metadata file designed to provide summary data to tools (e.g. the [Flight Emissions tool](https://patricklake2.github.io/flight-emissions/) displays today's total kg of CO2, total flights, total kilometres flown, and unique destinations) and let them know which dates have more data available for further breakdown. It is expected to be updated on a daily basis.

```javascript
{
  "directory": "https://raw.githubusercontent.com/patricklake2/flight-emissions/master/leeds-bradford/data/",
  "lastupdate": "2020-01-08",
  "dates": ["2019-12-11","2019-12-12","2019-12-13","2019-12-14","2019-12-20","2020-01-07"]
  "emissions": [135000,130000,125000,140000,139000,135628]
  "flights": [19,17,16,21,20,19]
}
```
The format is defined as follows:

* `directory` - the directory where the individual data files for each day are stored
* `lastupdate` - the ISO 8601 date this was last updated
* `dates` - a chronologically ordered array of ISO 8601 dates (local time) for which individual data files exist. These allow URLs to be constructed to get the breakdown for specific days
* `emissions` - an array (in the same order as `dates`) with the total emissions in kilograms
* `flights` - an array (in the same order as `dates`) with the number of flights per day

We've used ordered arrays so that more parameters could be added in the future, if necessary. With `dates`, `emissions`, and `flights` the file should grow by around 25 bytes per day (~9kB per year).

## Primary index

Once you've got your flight data and airport metadata files published somewhere on the web (with CORS enabled), you just need to add the location of your airport metadata file to our [our Primary index file](../index.json). You can edit it yourself and open a pull request, or [open an issue](https://github.com/odileeds/flight-data/issues/new) and we'll help. This file will let tools know which airports are available. It shouldn't need to be updated often.

```javascript
[
	{ "IATA":"LBA", "ICAO":"EGNM", "n":"Leeds Bradford", "author":"Patrick Lake", "url":"https://github.com/patricklake2/flight-emissions/", "index":"https://raw.githubusercontent.com/patricklake2/flight-emissions/master/leeds-bradford/data/index.json" },
	{ "IATA":"MAN", "ICAO":"EGCC", "n":"Manchester", "author":"TBD", "repo":"", "index":"" }
]
```

Each airport then contains:

* `IATA` - the three letter IATA code (if it exists)
* `ICAO` - the ICOA code (if it exists)
* `n` - a free-form field for the displayed airport name
* `author` - the name of the person compiling emissions data for this airport
* `url` - a URL to e.g. the Github repository
* `index` - the URL of an `index.json` file that defines the [metadata](#airport-metadata) for this airport
