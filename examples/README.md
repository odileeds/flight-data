# Data

## Flight Data
You should have one directory for your data, in it there should be seperate JSON file for each day, where the filename is the date in ISO format (YYYY-MM-DD.json).
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

We also have a [full example](flights-example.json) to refer to.

## Metadata
(format to be defined)

## Index File
Once you're up and running, you just need to add the location of your metadata file to our [our index file.](data/index.json)  You can edit it yourself and open a pull request, or [open an issue](https://github.com/odileeds/flight-data/issues/new) and we'll help.


