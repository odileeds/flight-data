# Data

* [Flight data](#flight-data)
* [Metadata](#metadata)
* [Index file](#index-file)

## Flight data

You should have one directory for your data, in it there should be seperate JSON file for each *local time* day, where the filename is the date in ISO format (YYYY-MM-DD.json).
The file should have the following format:

```javascript
{
	"from": {        // origin airport information
		"n": "Leeds Bradford",
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
		"to":{"n":"Amsterdam","IATA":"AMS","geo":[4.7639,52.3086],"cc":"NL","continent":"EU" },
		"km":462.46,
		"emissions":{"f":10.2,"kg":4717.13}
	},{
		//another flight 
	}]
}
```

* `from` - the origin airport and should appear once per file. This helps if the file ends up on a different file system.
  * `n` - a free-form field for the displayed airport name
  * `IATA` - the international airport code
  * `geo` - a [`longitude`,`latitude`] array with the coordinates in degrees (compatible with GeoJSON)
  * `cc` - the two letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code for the origin
  * `continent` - a [two letter continent code](https://datahub.io/core/continent-codes) e.g. `AF`, `NA`, `OC`, `AN`, `AS`, `EU`, `SA`
* `flights` - an array of flights (in chronological order). Each flight contains:
  * `id` - the flight code
  * `time` - the [ISO8601 timestamp](https://en.wikipedia.org/wiki/ISO_8601) for the departure (with timezone)
  * `airline` - the name of the airline
  * `aircraft` - details of the aircraft broken down into:
    * `code` - the aircraft code e.g. "E190"
    * `name` - the aircraft name e.g. "EMBRAER ERJ190"
  * `to` - the destination broken down into:
    * `n` - a free-form field for the displayed airport name
    * `IATA` - the international airport code
    * `geo` - a [`longitude`,`latitude`] array with the coordinates in degrees (compatible with GeoJSON)
    * `cc` - the two letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code for the origin
    * `continent` - a [two letter continent code](https://datahub.io/core/continent-codes) e.g. `AF`, `NA`, `OC`, `AN`, `AS`, `EU`, `SA`
  * `km` - the great circle distance in kilometres
  * `emissions` - the estimated CO2 emissions of the flight
    * `f` - the emissions factor
    * `kg` - the kilograms of CO2

## Metadata

```javascript
{
  "directory": "https://raw.githubusercontent.com/patricklake2/flight-emissions/master/leeds-bradford/data/",
  "lastupdate": "2020-01-08",
  "dates": ["2019-12-11","2019-12-12","2019-12-13","2019-12-14","2019-12-20","2020-01-07"]
  "emissions": [135000,130000,125000,140000,139000,135628]
  "flights": [19,17,16,21,20,19]
}
```

The metadata file for an individual airport is designed to provide summary data to tools and let them know which dates have more data available. The format is defined as follows:

* `directory` - the directory where the individual data files for each day are stored
* `lastupdate` - the ISO 8601 date this was last updated
* `dates` - a chronologically ordered array of ISO 8601 dates (local time) for which individual data files exist. These allow URLs to be constructed to get the breakdown for specific days
* `emissions` - an array (in the same order as `dates`) with the total emissions in kilograms
* `flights` - an array (in the same order as `dates`) with the number of flights per day

We've used ordered arrays so that more parameters could be added in the future, if necessary. With `dates`, `emissions`, and `flights` the file should grow by around 25 bytes per day (~9kB per year).

## Index File

Once you're up and running, you just need to add the location of your metadata file to our [our index file](data/index.json).  You can edit it yourself and open a pull request, or [open an issue](https://github.com/odileeds/flight-data/issues/new) and we'll help.

```javascript
{
	"LBA":{ "n":"Leeds Bradford", "author":"Patrick Lake", "url":"https://github.com/patricklake2/flight-emissions/", "index":"https://raw.githubusercontent.com/patricklake2/flight-emissions/master/leeds-bradford/data/index.json" },
	"MAN":{ "n":"Manchester", "author":"TBD", "repo":"", "index":"" }
}
```

Each airport (keyed by the IATA code) then contains:

* `n` - a free-form field for the displayed airport name
* `author` - the name of the person compiling emissions data for this airport
* `url` - a URL to e.g. the Github repository
* `index` - the URL of an `index.json` file that defines the metadata for this airport
