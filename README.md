# JSON Demo

The data here is from the National Weather Service's [API Web Service](https://www.weather.gov/documentation/services-web-api). Here is the basic usage. 

If you happen to know the NWS Weather Forcase Office Grid for your forecast, you can git your forcast using the following URL _endpoint_:

`https://api.weather.gov/gridpoints/{office}/{gridX},{gridY}/forecast`

If you don't know your `office`, `gridX`, and `gridY` values (and why would you?), then you can use a separate call to a different endpoint to get the link to your forcast, assuming you knew your longitude and latitude:

`https://api.weather.gov/points/{latitude},{longitude}`

For instance, here is the [link for Avenues](https://api.weather.gov/points/40.7494,-74.0059) whose latitude/logitude coordinates are 40.7494,-74.0059. If you used your web browser to go to this link, you'd see the contents of a JSON file. The structure relevant structure is:

```JSON
{
  ...other top-level properties
  "properties" : {
    "@id":"https://api.weather.gov/points/40.7494,-74.0059", // the original call
    ... other properties
    "gridId": "OKX",
    "gridX": 33,
    "gridY": 37,
    "forecast": "https://api.weather.gov/gridpoints/OKX/33,37/forecast",
    ... more properties
  }
}
```

Thus, the link we need is: `https://api.weather.gov/gridpoints/OKX/33,37/forecast`

## Side note

It should be noted here that while we are doing this work manually, you _should_ be able to see how we could automate this process. Assuming we could somehow get a latitude/longitude combination, we could use "informational" link to then get the forecast link and then use that to get the data. 

---

Once we have our link, we can finally make use of it (again, in `preload`) like so:

```JavaScript
function preload() {
  weatherJSON = loadJSON("https://api.weather.gov/gridpoints/OKX/33,37/forecast")  
}
```

and `weatherJSON` becomes an object we can navigate. Again, if we opned the link in a browser we would see a structure like so:

```JSON
{
  ... top level properties
  "properties": {
    ... other properties
    "periods": [
      ... a bunch of objects representing "forecast periods" whose shape is like so:
      {
        "number":1,
        ...other data
        "isDaytime":true,
        "temperature":39,
        "windSpeed": "12 mph"
        ...a bunch of other data
      },
      ... more objects
    ]
  }
}
```

From this point,  it is simply a matter of choosing what data we want to display and how, and then navigating the array in much the same fashion we have already been doing. 
