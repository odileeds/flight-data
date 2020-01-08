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
We also have a [full example](flights-example.json) to refer to.

## Metadata
(format to be defined)

## Index File
Once you're up and running, you just need to add the location of your metadata file to our [our index file.](data/index.json)  You can edit it yourself and open a pull request, or [open an issue](https://github.com/odileeds/flight-data/issues/new) and we'll help.


