DarkSky 
==========

[![CircleCI](https://circleci.com/gh/Detrous/darksky/tree/master.svg?style=svg)](https://circleci.com/gh/Detrous/darksky/tree/master) [![CircleCI](https://codecov.io/gh/detrous/darksky/branch/master/graph/badge.svg)](https://codecov.io/gh/detrous/darksky/tree/master)

This  library for the [Dark Sky
API](https://darksky.net/dev/docs) provides access to detailed
weather information from around the globe.

* [Installation](#installation)
* [Get started](#get-started)
* [Contact us](#contact-us)
* [License](#license)


### Installation
```
pip3 install darksky_weather
```

### Get started

Before you start using this library, you need to get your API key
[here](https://darksky.net/dev/register).

All classes are fully annotated, source code it's your best doc : )

```python
from darksky.api import DarkSky, DarkSkyAsync
from darksky.types import languages, units, weather


API_KEY = 'f40beeca4915d42a90c66737b1465346'

# Synchronous way
darksky = DarkSky(API_KEY)

latitude = 50.8333
longitude = -0.6332
forecast = darksky.get_forecast(
    latitude, longitude,
    extend=False, # default `False`
    lang=languages.ENGLISH, # default `ENGLISH`
    values_units=units.AUTO, # default `auto`
    exclude=[weather.MINUTELY, weather.ALERTS, weather.hourly, weather.currently], # default `[]`,
    timezone='UTC' # default None - will be set by DarkSky API automatically
)

# Synchronous way Time Machine 

from datetime import datetime as dt

darksky = DarkSky(API_KEY)
t = dt(2010, 2, 6, 12)

latitude = 50.8333
longitude = -0.6332
forecast = darksky.get_time_machine_forecast(
    latitude, longitude,
    extend=False, # default `False`
    lang=languages.ENGLISH, # default `ENGLISH`
    values_units=units.AUTO, # default `auto`
    exclude=[weather.MINUTELY, weather.ALERTS, weather.hourly, weather.currently], # default `[]`,
    timezone='UTC', # default None - will be set by DarkSky API automatically
    time=t
)

# Asynchronous way
# NOTE! On Mac os you will have problem with ssl checking https://github.com/aio-libs/aiohttp/issues/2822
# So you need to create your own session with disabled ssl verify and pass it into the get_forecast
# session = aiohttp.ClientSession(connector=aiohttp.TCPConnector(verify_ssl=False))
# darksky = DarkSkyAsync(API_KEY)
# forecast = await darksky.get_forecast(
#     *arguments*,
#     client_session=session
# )

darksky = DarkSkyAsync(API_KEY)

latitude = 50.8333
longitude = -0.6332
forecast = await darksky.get_forecast(
    latitude, longitude,
    extend=False, # default `False`
    lang=languages.ENGLISH, # default `ENGLISH`
    values_units=units.AUTO, # default `auto`
    exclude=[weather.MINUTELY, weather.ALERTS, weather.HOURLY, weather.CURRENTLY], # default `[]`
    timezone='UTC' # default None - will be set by DarkSky API automatically,
    client_session=aiohttp.ClientSession() # default aiohttp.ClientSession()
)

# Final wrapper identical for both ways
forecast.latitude # 42.3601
forecast.longitude # -71.0589
forecast.timezone # timezone for coordinates. For exmaple: `America/New_York`

forecast.currently # CurrentlyForecast. Can be found at darksky/forecast.py
forecast.minutely # MinutelyForecast. Can be found at darksky/forecast.py
forecast.hourly # HourlyForecast. Can be found at darksky/forecast.py
forecast.daily # DailyForecast. Can be found at darksky/forecast.py
forecast.alerts # [Alert]. Can be found at darksky/forecast.py
```

### Contact us.

#If you have any issues or questions regarding the library, you are welcome to create an issue, or
#You can write an Email to `detrous@protonmail.com`


### License.

Library is released under the [MIT License](./LICENSE).
