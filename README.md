# homebridge-weather-plus
[![npm](https://img.shields.io/npm/v/homebridge-weather-plus.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-weather-plus)
[![npm](https://img.shields.io/npm/dt/homebridge-weather-plus.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-weather-plus)
[![GitHub last commit](https://img.shields.io/github/last-commit/naofireblade/homebridge-weather-plus.svg?style=flat-square)](https://github.com/naofireblade/homebridge-weather-plus)
[![Weather](https://img.shields.io/badge/weather-sunny-edd100.svg?style=flat-square)](https://github.com/naofireblade/homebridge-weather-plus)

This is a weather plugin for [homebridge](https://github.com/nfarina/homebridge) that features current observations, daily forecasts and history graphs for multiple locations and services. You can download it via [npm](https://www.npmjs.com/package/homebridge-weather-plus).  

If you like this plugin, I would be very grateful for your support:

<a href="https://www.buymeacoffee.com/2D1nUuK36" target="_blank"><img width="140" src="https://bmc-cdn.nyc3.digitaloceanspaces.com/BMC-button-images/custom_images/orange_img.png" alt="Buy Me A Coffee"></a>

Feel free to leave any feedback [here](https://github.com/naofireblade/homebridge-weather-plus/issues).

## Features
- Get current observations and forecasts for up to 10 days
- Choose from 4 different weather [services](#choose-your-weather-service)
- Add [multiple](#multiple-stations-configuration) locations/services
- See the weather [history](#screenshots) in the Eve App
- See translations and [icons](#screenshots) in the Eve App
- Use all values in HomeKit rules with the Eve App

## Observations and Forecasts

The following **19 observation and forecast values** can be displayed and used in HomeKit rules.

- Air Pressure
- Cloud Cover
- Condition
- Condition Category (Sun = 0, Clouds = 1, Rain = 2, Snow = 3)
- Dew Point
- Humidity
- Ozone
- Rain Last Hour
- Rain All Day
- Rain Chance
- Solar Radiation
- Temperature
- Temperature Min
- Temperature Max
- UV-Index
- Visibility
- Wind Direction
- Wind Speed
- Wind Speed Maximum
- *Observation Time*
- *Observation Station*
- *Forecast day*

## Choose your Weather Service

This plugin supports multiple weather services. Each has it's own advantages. The following table shows a comparison to help you choosing one.

|                            |            Dark Sky (recommended)            |                   OpenWeatherMap                                 | Weather Underground (only if you can provide weather data in exchange)|
|----------------------------|:--------------------------------------------:|:----------------------------------------------------------------:|:----------------------------------------------------------------:|
| Current observation values |                      15                      |                                7                                 |                                12                                |
| Forecast values            |                      16                      |                                9                                 |                                 0                                |
| Forecast days              |                       7                      |                                 5                                |                                 0                                |
| Location                   |                geo-coordinates               |                city name, city id, geo-coordinates               |                           station id                             |
| Personal weather stations  |                      :x:                     |                        :heavy_check_mark:                        |                        :heavy_check_mark:                        |
| Free                       |               :heavy_check_mark:             |                        :heavy_check_mark:                        |           :heavy_check_mark: (only if you own a station)         |
| Register                   | [here](https://darksky.net/dev/register)     | [here](https://openweathermap.org/appid)                         | [here](https://www.wunderground.com/member/api-keys)             |

*You can add more services by forking the project and submitting a pull request.*

## Installation

1. Install homebridge using: `npm install -g homebridge`
2. Install this plugin using: `npm install -g homebridge-weather-plus` *Note: The installation might take 5 minutes.*
3. Gather an API key for a weather service from the register link in the table above
4. Update your configuration file. See the samples below.

## Configuration

Below are example configurations for all weather apis.

### Dark Sky

**key**  
The API key that you get by [registering](https://darksky.net/dev/register) for the Dark Sky service.

**locationGeo**  
List with the latitude and longitude for your location (don't forget the square brackets). You can get your coordinates: [here](http://www.mapcoordinates.net/en).

```json
"platforms": [
	{
		"platform": "WeatherPlus",
		"name": "WeatherPlus",
		"service": "darksky",
		"key": "YOUR_API_KEY",
		"locationGeo": [52.5200066, 13.404954]
	}
]
```

### OpenWeatherMap

**key**  
The API key that you get by [registering](https://openweathermap.org/appid) for the Dark Sky service.

**location**<sup>[1](#a1)</sup>  
Numerical city id, can be found [here](https://openweathermap.org/find).

**locationCity**<sup>[1](#a1)</sup>  
City name and optional country code, can be found [here](https://openweathermap.org/find).

**locationGeo**<sup>[1](#a1)</sup>  
List with the latitude and longitude for your location (don't forget the square brackets). You can get your coordinates: [here](http://www.mapcoordinates.net/en).

<b name="a1">1</b> You need only **one** of the location options.
****

```json
"platforms": [
	{
		"platform": "WeatherPlus",
		"name": "WeatherPlus",
		"service": "openweathermap",
		"key": "YOUR_API_KEY",
		"location": 2950159,
		"locationCity": "Berlin, DE",
		"locationGeo": [52.5200066, 13.404954]
	}
]
```

### Weather Underground

Since March 2019 you need to register your own weather station with Weather Underground to get weather data in exchange. After you registered your weather device ([here](https://www.wunderground.com/member/devices)), you can use the API.

**key**  
The API key that you get by [registering](https://www.wunderground.com/member/api-keys) for the Weather Underground service.

**location**  
Your personal StationID.

```json
"platforms": [
    {
        "platform": "WeatherPlus",
        "name": "WeatherPlus",
        "service": "weatherunderground",
        "key": "YOUR_API_KEY",
        "location": "YOUR_STATION_ID"
    }
]
```

## Advanced Configuration

Below are explanations for a lot of advanced parameters to adjust the plugin to your needs. All parameters are *optional*.

**forecast**  
List of forecast days with 1 for today, 2 for tomorrow etc. Default are none `[]`. Maximum depends on the choosen [weather service](#choose-your-weather-service).

**interval**  
Update interval in minutes. The default is `4` minutes because the rate for free API keys is limited.

**units**  
Conversions used for reporting values. The default is `"metric"`. The options are:  
`"si"` or `"metric"`  
`"us"` or `"imperial"`  
`"ca"` to report wind speeds in km/h instead of m/s  
`"uk"` to report visibility in miles and wind speeds in km/h instead of m/s

**language**  
Translation for the current day and the weather report. Available languages can be found [here](https://github.com/darkskyapp/translations/tree/master/lib/lang). The default is `en`.

**displayName**  
Name for the current condition accessory. The default is `"Now"`. You can also set this to your city name.

**displayNameForecast**  
Name for the forecast accessories. Adds a prefix to the forecast days.

**currentObservations**  
parameter is *optional* and sets how the 3 current observations temperature, humidity and pressure are displayed. You can choose one of these 2 options:  
`"eve"` (this combines all 3 values into one row in the eve app but shows nothing in the Apple Home app)  
`"normal"` (default, this shows all 3 values in a seperate row in the eve app and shows the temperature in the Apple Home app)  

The **fakegatoParameters** parameter is *optional*. By default, history is persisted on filesystem. You can pass your own parameters to *fakegato-history* module using this paramter, in order to change the location of the persisted file or use GoogleDrive persistance. See https://github.com/simont77/fakegato-history#file-system and https://github.com/simont77/fakegato-history#google-drive for more info. **IMPORTANT NOTE:** Do not modify the parameter for the fakegato internal timer.

The **serial** parameter is *optional* and sets the Serial Number of the accessory. If it's not provided the serial number will be set to the **location** if present, or to 999 if not. Note that for proper operation of fakegato when multiple fakegato-enabled weather accessories are present in your system, the serial number must be unique.

### Example

```json
"platforms": [
        {
            "platform": "WeatherPlus",
            "name": "WeatherPlus",
            "forecast": [1,2],            
            "interval": 5,
            "units": "metric",
            "language": 
            "displayName":"Conditions OWM",
            "displayNameForecast":"Forecast OWM",
            "service": "openweathermap",
            "key": "XXXXXXXXXXXXXXX",
            
            "locationGeo": [45.4999952, 9.3061655],
            "serial": "OWM"                        
        }
    ]
```

## Multiple Stations Configuration

You can set up multiple stations for different locations and/or weather services by putting your configuration in an **stations** array. The parameters **interval** and **units** are global to all accessories, and must be set at the top level. 

**IMPORTANT NOTE:** If setting multiple weather accessories, ensure that each accessory has a unique name, or you will get an error from *homebridge*. If you do not set this parameter the plugin will take care of that.

**IMPORTANT NOTE:** If setting multiple weather accessories, ensure that each accessory has a unique name, or you will get an error from *homebridge*. If you do not set this parameter the plugin will take care of that.

### Example

```json
"platforms": [
        {
            "platform": "WeatherPlus",
            "name": "WeatherPlus",
            "interval": 5,
            "units": "si",
            "stations":[{
                "displayName":"Conditions OWM",
                "displayNameForecast":"Forecast OWM",
                "service": "openweathermap",
                "key": "YOUR_API_KEY",
                "forecast": [1,2],
                "locationGeo": [45.4999952, 9.3061655],
                "serial": "OWM" 
            },{
                "displayName":"Conditions DS",
                "displayNameForecast":"Forecast DS",
                "service": "darksky",
                "key": "YOUR_API_KEY",
                "forecast": [1,2],
                "locationGeo": [45.4999952, 9.3061655],
                "serial": "DS"
            }]
        }
    ]
```

## Example Use Cases

- Switch on a blue light in the morning when the chance for rain is above 20% today (or white when the forecast condition is snow / yellow when it's sunny).
- Start your automatic garden irrigation in the evening, depending on the amount of rain today and the forecast for tomorrow.

**Hint:** To trigger rules based on time and weather condition you will need a plugin like [homebridge-delay-switch](https://www.npmjs.com/package/homebridge-delay-switch). Create a dummy switch that resets after some seconds. Set this switch to on with a timed rule. Then create a condition rule that triggers when the switch goes on depending on weather conditions of your choice.

## Screenshots
![Current Conditions in Elgato Eve app](https://i.imgur.com/ql9t8w0l.png)
![History graph in Elgato Eve app](https://i.imgur.com/8opO7hel.png)
>(c) Screenshots are taken from the Elgato Eve app

## Contributors
Many thanks go to
- [Kevin Harwood](https://github.com/kcharwood) for his original homebridge-weather-station
- [Clark Endrizzi](https://github.com/cendrizzi) for his wundergroundnode library
- [simont77](https://github.com/simont77) for his fakegato-history library, the eve weather emulation, the multiple stations feature and several other great improvements
- [GatoPharaoh](https://github.com/GatoPharaoh) for his interval option pull request
- [David Werth](https://github.com/werthdavid) for integrating the OpenWeatherMap and Yahoo apis
- [Marshall T. Rose](https://github.com/mrose17) for adding support for imperial units and the displayName parameter
- [Bill Waggoner](https://github.com/ctgreybeard) for his fix for the crashing wunderground api
- [Russell Sonnenschein](https://github.com/ctgreybeard) for adding the new 2019 weatherunderground api

This plugin is a fork of [homebridge-weather-station](https://github.com/kcharwood/homebridge-weather-station) which is no longer being developed. That one is a fork of [homebridge-wunderground](https://www.npmjs.com/package/homebridge-wunderground).

## Attribution
- [Powered by Dark Sky](https://darksky.net/poweredby/)
- [Powered by Weather Underground](https://www.wunderground.com/)
- [Powered by OpenWeatherMap](https://openweathermap.org/)
- [Powered by Yahoo](https://yahoo.com/)
