# Data

## Flight Data
You should have one directory for your data, in it there should be seperate JSON file for each day, where the filename is the date in ISO format (YYYY-MM-DD.json).
The file should have the following format:

```javascript
{
    "Date": "2019-12-11",
    "Airport": {        //origin airport information
        "Name": "Leeds Bradford",
        "IATA": "LBA"
    },
    "Flights": [
        {
            "FlightNumber": "KL1540",
            "DateTime": "2019-12-11 06:10:00", // Should be YYYY-MM-DD HH:MM:SS format
            "Airline": "KLM",
            "Destination": "Amsterdam",
            "IATA": "AMS",
            "Lat": 52.3086,
            "Lon": 4.76389,
            "Country": "Netherlands",
            "Continent": "Europe",
            "Distance": 462.46,     //measured in kilometers
            "Flight_Type": "ShortHaul",
            "Aircraft_Code": "E190",        //leave empty if unknown
            "Aircraft_Name": "EMBRAER ERJ190",      //leave empty if unknown
            "Emissions_Factor": 10.2,     //kg CO2 per km
            "Emissions": 4717.13    //distance * emissions_factor
        },
        {
            //another flight 
        }
    ]
}
```
We also have a [full example](flights-example.json) to refer to.

## Metadata
In the directory where your data is stored, you should create a file named 'meta.json'. This is just so our website knows where to find your most recent data. It only requires 1 fields and should be in the following format:
```json
{
    "lastUpdated": "2019-11-31"
}
```

## Index File
Once you're up and running, either add yourself to [our index file](data/index.json) and open a pull request, or open an issue and we'll help.


