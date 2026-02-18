# Overview
This tutorial demonstrates how to use **cURL** to retrieve real-time wave and wind data from the Storm Glass API and how to interpret the resulting **JSON** payload.

# Process
## 1. The Request (cURL)
To fetch marine data, you will use a GET request. We will specify coordinates for Bondi Beach (Lat: -33.89, Lng: 151.27) and limit our parameters to wave height and wind speed.

**Command**:
```
curl -X GET "https://api.stormglass.io/v2/weather/point?lat=-33.890&lng=151.274&params=waveHeight,windSpeed" \
     -H "Authorization: YOUR_API_KEY"
```
**Header Breakdown**:

* `-X GET`: Specifies the HTTP method.

* `-H "Authorization: ..."`: Passes your API key securely in the header rather than the URL string.

* `params=waveHeight,windSpeed`: Filters the response to return only the data points we need.

## 2. The Response (JSON)
The API returns a JSON object containing an hours array. Each object in that array represents a one-hour time slice.

**Sample Payload**:
```
{
  "hours": [
    {
      "time": "2026-02-17T21:00:00+00:00",
      "waveHeight": {
        "noaa": 1.2,
        "sg": 1.4,
        "dwd": 1.3
      },
      "windSpeed": {
        "noaa": 4.5,
        "sg": 4.8
      }
    }
  ],
  "meta": {
    "cost": 1,
    "dailyQuota": 50,
    "lat": -33.89,
    "lng": 151.27
  }
}
```

### 3. Interpreting the Data
When parsing the JSON response, keep the following in mind:
* **Multi-Source Data**: Storm Glass provides data from multiple sources (e.g., `noaa`, `dwd`). For general surf reports, the `sg` (Storm Glass internal) value is the recommended consolidated metric.
* **Units**: By default, `waveHeight` is returned in meters and `windSpeed` in meters per second.
* **The Meta Object**: Always check the `meta` object at the bottom of the JSON. It tells you the "cost" of the request and how much of your `dailyQuota` remains.

## 4. Common Error Responses
If your cURL command fails, the API will return a JSON error object:
|Status Code|Reason|JSON Response Snippet|
|---|---|---|
|401|Invalid API Key|`{"errors": {"key": "Invalid API key"}}`|
|402|Quota Exceeded|`{"errors": {"quota": "Daily quota reached"}}`|
422|Bad Coordinates|`{"errors": {"lat": "Latitude is required"}}`|