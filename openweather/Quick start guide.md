# Overview

The **Current Weather API** provides real-time weather data (temperature, humidity, wind, etc.) for any location on Earth using city names or geographic coordinates.

# Prerequisites

1. **Sign Up**: Create a free account at [OpenWeatherMap](https://openweathermap.org).
2. **Get API Key**: Navigate to your **API Keys** tab and copy your default key. The key is also sent to the registered email address after sign up.
> **Note**: Free API keys can take up to 2 hours to activate after registration.
3. **Tooling**: Ensure you have `curl` installed or use [Postman](https://www.postman.com/).

# Process
## Step 1: Authentication
OpenWeatherMap uses **Query Parameter Authentication**. Every request must include your API key in the `appid` parameter:

Base URL: `https://api.openweathermap.org/data/2.5/weather`

>**Caution**: Keep your API key secret. Do not commit it to public repositories.

## Step 2: Make Your First Request
This example retrieves the current weather for Bangalore, Karnataka. Copy and run the `cURL` command below into your terminal, replacing {API_key} with your actual key.
```
curl -X GET "https://api.openweathermap.org/data/2.5/weather?q=Bangalore&appid={API_key}&units=metric"
```

## Step 3: Verify the Response 
A successful request returns a **200** status and a JSON body. Here are the key fields you will likely need:
```
{
    "coord": {
        "lon": 77.6033,
        "lat": 12.9762
    },
    "weather": [
        {
            "id": 800,
            "main": "Clear",
            "description": "clear sky",
            "icon": "01d"
        }
    ],
    "base": "stations",
    "main": {
        "temp": 83.43,
        "feels_like": 81.55,
        "temp_min": 80.85,
        "temp_max": 84.49,
        "pressure": 1009,
        "humidity": 30,
        "sea_level": 1009,
        "grnd_level": 912
    },
    "visibility": 8000,
    "wind": {
        "speed": 20,
        "deg": 68,
        "gust": 52.01
    },
    "clouds": {
        "all": 6
    },
    "dt": 1771326480,
    "sys": {
        "type": 2,
        "id": 2036502,
        "country": "IN",
        "sunrise": 1771290695,
        "sunset": 1771332956
    },
    "timezone": 19800,
    "id": 1277333,
    "name": "Bengaluru",
    "cod": 200
}
```
# Common Troubleshooting
|Status Code | Meaning | Solution|
|-----|-----|-----|
401|Unauthorized|Your API key is invalid or not yet activated (wait 2 hours).|
404|Not Found|The coordinates or city name provided do not exist in our database.
429|Too Many Requests|You have exceeded the free limit of 60 calls per minute.