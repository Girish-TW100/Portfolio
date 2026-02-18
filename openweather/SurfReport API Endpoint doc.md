# SurfReport API
## Overview
The API provides details regarding beach conditions, such as surf height, wind speed, water temperature, and tide, which may be necessary for surfers. It also offers suggestions on whether to proceed with surfing.
## Endpoints and methods
`Get` /surfreport/**{beachId}**
gets the surf report for a specific beach ID.
## Parameters
### Path Parameters
|Parameter| Description|
|----------|------------|
|**{beachId}**|The ID of the beach you want to get surfing conditions about. Valid **beachId** can be  retrieved from a list of beaches on our site [example](https://example.com).

### Query String Parameters
| Parameter|Data Type|Required/Optional|Description|
|----|----|----|----|
|days|integer|optional|The number of days you want to get in the response. Default number is 3.
|time|Integer. Unix format (ms since 1970) in UTC|optional|If you include time in hours, then only the entered hour data will be returned in response.
## Sample Request
`curl -I -X GET "https://api.openweathermap.org/data/2.5/surfreport?zip=95050&appid=APIKEY&units=imperial&days=2"`
## Sample Response
```
    {
    "surfreport": [
        "beach": "santa cruz",
        "monday": {
            "1pm": {
                "tide": 5,
                "wind": 15,
                "watertemp": 80,
                "surfheight": 5,
                "recommendation": "Go surfing!"
            },
            "2pm":{
              "tide": -1,
                "wind": 15,
                "watertemp": 80,
                "surfheight": 4,
                "recommendation": "Surfing conditions are okay, not great."  
            },
            "3pm":{
                "tide": -1,
                "wind": 15,
                "watertemp": 56,
                "surfheight": 1,
                "recommendation": "Not a good day for surfing." 
                }
                ...
            }
        ]
    }
```
### Response Definitions

The following table describes each item in the response.
|Response item|Description|Data Type|
|---|---|---|
|**beach**|The beach name you entered based on the beachID. Although most beaches are supported, check for the beaches available at our website [example](example.com)|string|
|**{day}**|The day of the week selected. A maximum of 7 days data is returned in the response. By default data of only 3 days is returned.|integer|
**{time}**|The time of the day. Only report for the entered time will be returned in response.|integer
**{tide}**|The level of tide at the beach on a selected day in feets. It is the distance the inland water rises to and it can be a positive or negative number. Negative numbers represent incoming tide and a positive number indicates outgoing tide. A 0(zero) indicates a restful tide which is neither coming in or going out. The report won't include any details about riptide conditions. To switch from feet to metrics, add a query string of ``&units=metrics``. Default is ``&units=imperial``. |integer
**{wind}**|The wind speed at the beach measured in knots.
**{watertemp}**|	The temperature of the water, returned in Fahrenheit or Celsius depending upon the units you specify. Water temperatures below 70 F usually require you to wear a wetsuit. With temperatures below 60, you will need at least a 3mm wetsuit and preferably booties to stay warm.|integer
**{surfheight}**|The height of the waves, returned in either feet or centimeters depending on the units you specify. A surf height of 3 feet is the minimum size needed for surfing. If the surf height exceeds 10 feet, it is not safe to surf.|integer
**{Recommendation}**|An overall recommendation based on a combination of the various factors (wind, watertemp, surfheight). Three responses are possible: (1) "Go surfing!", (2) "Surfing conditions are okay, not great", and (3) "Not a good day for surfing." Each of the three factors is scored with a maximum of 33.33 points, depending on the ideal for each element. The three elements are combined to form a percentage. 0% to 59% yields response 3, 60% - 80% and below yields response 2, and 81% to 100% yields response 1.|string
