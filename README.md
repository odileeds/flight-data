# Flight emissions data

A distributed project to collate flight data for emissions calculations.

## How to contribute?

If you'd like to take on responsibility for recording data for a specific airport, you will need to:

* save daily JSON files of flights (format to be defined)
* create an index JSON file that describes your dataset (format to be defined)
* update the [index.json file](data/index.json) to include your airport


## Example - Leeds Bradford

We've created a Python script which takes daily departures data from Leeds Bradford Airport's website. There's more information on how this works, and how it could be adapted, in the leeds-bradford folder.