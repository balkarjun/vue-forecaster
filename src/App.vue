<template>
  <main>
    <section class="info">
      <div class="top">
        <div class="inputbar">
          <i class="fas fa-map-marker-alt"></i>
          <input type="text" value="Tokyo, Japan">
        </div>
      </div>
      <div class="middle">
        {{ forecasts[0].temperature }}
      </div>
      <div class="bottom">
        <div>
          <p>{{ forecasts[0].feelsLike }}</p>
          <p>Feels Like</p>
        </div>
        <div>
          <p>{{ forecasts[0].precipChance }}%</p>
          <p>Chance of Rain</p>
        </div>
        <div>
          <p>{{ forecasts[0].uvIndex }}</p>
          <p>UV Index</p>
        </div>
      </div>
    </section>
    <section>
      <p class="summary">{{ forecasts[0].summary }}</p>
      <div class="forecast-list">
        <div class="forecast" v-for="(item, index) in forecasts" :key="index">
          {{ item.min }} - {{ item.max }}
          {{ item.icon }}
        </div>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      forecasts: null
    }
  },
  created() {
    // https://api.darksky.net/forecast/[key]/[latitude],[longitude]
    const location = '50,0'
    const key = '85e8f9c3f0ba466ca2512a8008d0df8e';
    const proxy = 'https://cors-anywhere.herokuapp.com/';
    const exclude = '[minutely,hourly,alerts,flags]';
    const url = `${proxy}https://api.darksky.net/forecast/${key}/${location}?exclude=${exclude}`;

    const res = {
      "latitude": 50,
      "longitude": 0,
      "timezone": "Europe/London",
      "currently": {
        "time": 1566383344,
        "summary": "Overcast",
        "icon": "cloudy",
        "nearestStormDistance": 67,
        "nearestStormBearing": 351,
        "precipIntensity": 0,
        "precipProbability": 0,
        "temperature": 63.18,
        "apparentTemperature": 63.18,
        "dewPoint": 54.3,
        "humidity": 0.73,
        "pressure": 1028.32,
        "windSpeed": 6.23,
        "windGust": 6.36,
        "windBearing": 221,
        "cloudCover": 1,
        "uvIndex": 3,
        "visibility": 10,
        "ozone": 334.7
      },
      "daily": {
        "summary": "No precipitation throughout the week, with high temperatures rising to 71°F on Saturday.",
        "icon": "clear-day",
        "data": [
          {
            "time": 1566342000,
            "summary": "Mostly cloudy throughout the day.",
            "icon": "partly-cloudy-day",
            "sunriseTime": 1566363590,
            "sunsetTime": 1566414572,
            "moonPhase": 0.69,
            "precipIntensity": 0.0001,
            "precipIntensityMax": 0.0002,
            "precipIntensityMaxTime": 1566428400,
            "precipProbability": 0.04,
            "precipType": "rain",
            "temperatureHigh": 64.58,
            "temperatureHighTime": 1566406800,
            "temperatureLow": 63.15,
            "temperatureLowTime": 1566442800,
            "apparentTemperatureHigh": 64.58,
            "apparentTemperatureHighTime": 1566406800,
            "apparentTemperatureLow": 63.15,
            "apparentTemperatureLowTime": 1566442800,
            "dewPoint": 53.93,
            "humidity": 0.72,
            "pressure": 1027.31,
            "windSpeed": 5.46,
            "windGust": 10.21,
            "windGustTime": 1566342000,
            "windBearing": 257,
            "cloudCover": 0.36,
            "uvIndex": 4,
            "uvIndexTime": 1566396000,
            "visibility": 10,
            "ozone": 328.6,
            "temperatureMin": 61.51,
            "temperatureMinTime": 1566349200,
            "temperatureMax": 64.58,
            "temperatureMaxTime": 1566406800,
            "apparentTemperatureMin": 61.51,
            "apparentTemperatureMinTime": 1566349200,
            "apparentTemperatureMax": 64.58,
            "apparentTemperatureMaxTime": 1566406800
          },
          {
            "time": 1566428400,
            "summary": "Partly cloudy throughout the day.",
            "icon": "partly-cloudy-day",
            "sunriseTime": 1566450080,
            "sunsetTime": 1566500853,
            "moonPhase": 0.72,
            "precipIntensity": 0.0001,
            "precipIntensityMax": 0.0004,
            "precipIntensityMaxTime": 1566493200,
            "precipProbability": 0.05,
            "precipType": "rain",
            "temperatureHigh": 66.08,
            "temperatureHighTime": 1566493200,
            "temperatureLow": 63.65,
            "temperatureLowTime": 1566525600,
            "apparentTemperatureHigh": 66.08,
            "apparentTemperatureHighTime": 1566493200,
            "apparentTemperatureLow": 63.65,
            "apparentTemperatureLowTime": 1566525600,
            "dewPoint": 55.5,
            "humidity": 0.73,
            "pressure": 1025.82,
            "windSpeed": 3.94,
            "windGust": 7.47,
            "windGustTime": 1566489600,
            "windBearing": 5,
            "cloudCover": 0.13,
            "uvIndex": 5,
            "uvIndexTime": 1566478800,
            "visibility": 10,
            "ozone": 308.8,
            "temperatureMin": 63.15,
            "temperatureMinTime": 1566442800,
            "temperatureMax": 66.08,
            "temperatureMaxTime": 1566493200,
            "apparentTemperatureMin": 63.15,
            "apparentTemperatureMinTime": 1566442800,
            "apparentTemperatureMax": 66.08,
            "apparentTemperatureMaxTime": 1566493200
          },
          {
            "time": 1566514800,
            "summary": "Mostly cloudy throughout the day.",
            "icon": "partly-cloudy-day",
            "sunriseTime": 1566536570,
            "sunsetTime": 1566587134,
            "moonPhase": 0.76,
            "precipIntensity": 0.0001,
            "precipIntensityMax": 0.0003,
            "precipIntensityMaxTime": 1566590400,
            "precipProbability": 0.02,
            "precipType": "rain",
            "temperatureHigh": 67.93,
            "temperatureHighTime": 1566583200,
            "temperatureLow": 66.66,
            "temperatureLowTime": 1566612000,
            "apparentTemperatureHigh": 67.93,
            "apparentTemperatureHighTime": 1566583200,
            "apparentTemperatureLow": 66.66,
            "apparentTemperatureLowTime": 1566612000,
            "dewPoint": 57.57,
            "humidity": 0.75,
            "pressure": 1023.6,
            "windSpeed": 8.61,
            "windGust": 18.42,
            "windGustTime": 1566601200,
            "windBearing": 61,
            "cloudCover": 0.31,
            "uvIndex": 5,
            "uvIndexTime": 1566565200,
            "visibility": 10,
            "ozone": 303.2,
            "temperatureMin": 63.65,
            "temperatureMinTime": 1566525600,
            "temperatureMax": 67.95,
            "temperatureMaxTime": 1566586800,
            "apparentTemperatureMin": 63.65,
            "apparentTemperatureMinTime": 1566525600,
            "apparentTemperatureMax": 67.95,
            "apparentTemperatureMaxTime": 1566586800
          },
          {
            "time": 1566601200,
            "summary": "Mostly cloudy throughout the day.",
            "icon": "cloudy",
            "sunriseTime": 1566623059,
            "sunsetTime": 1566673413,
            "moonPhase": 0.79,
            "precipIntensity": 0.0001,
            "precipIntensityMax": 0.0004,
            "precipIntensityMaxTime": 1566622800,
            "precipProbability": 0.03,
            "precipType": "rain",
            "temperatureHigh": 71.39,
            "temperatureHighTime": 1566669600,
            "temperatureLow": 69.01,
            "temperatureLowTime": 1566716400,
            "apparentTemperatureHigh": 71.55,
            "apparentTemperatureHighTime": 1566669600,
            "apparentTemperatureLow": 69.34,
            "apparentTemperatureLowTime": 1566716400,
            "dewPoint": 60.66,
            "humidity": 0.75,
            "pressure": 1017.46,
            "windSpeed": 11.83,
            "windGust": 18.82,
            "windGustTime": 1566604800,
            "windBearing": 88,
            "cloudCover": 0.74,
            "uvIndex": 4,
            "uvIndexTime": 1566648000,
            "visibility": 10,
            "ozone": 300.8,
            "temperatureMin": 66.66,
            "temperatureMinTime": 1566612000,
            "temperatureMax": 71.39,
            "temperatureMaxTime": 1566669600,
            "apparentTemperatureMin": 66.66,
            "apparentTemperatureMinTime": 1566612000,
            "apparentTemperatureMax": 71.55,
            "apparentTemperatureMaxTime": 1566669600
          },
          {
            "time": 1566687600,
            "summary": "Mostly cloudy throughout the day.",
            "icon": "partly-cloudy-day",
            "sunriseTime": 1566709549,
            "sunsetTime": 1566759691,
            "moonPhase": 0.82,
            "precipIntensity": 0.0002,
            "precipIntensityMax": 0.0004,
            "precipIntensityMaxTime": 1566727200,
            "precipProbability": 0.11,
            "precipType": "rain",
            "temperatureHigh": 69.38,
            "temperatureHighTime": 1566712800,
            "temperatureLow": 65.75,
            "temperatureLowTime": 1566802800,
            "apparentTemperatureHigh": 69.57,
            "apparentTemperatureHighTime": 1566712800,
            "apparentTemperatureLow": 66.09,
            "apparentTemperatureLowTime": 1566802800,
            "dewPoint": 61,
            "humidity": 0.77,
            "pressure": 1015.51,
            "windSpeed": 10.21,
            "windGust": 15.18,
            "windGustTime": 1566687600,
            "windBearing": 211,
            "cloudCover": 0.67,
            "uvIndex": 4,
            "uvIndexTime": 1566734400,
            "visibility": 10,
            "ozone": 297.1,
            "temperatureMin": 66.53,
            "temperatureMinTime": 1566774000,
            "temperatureMax": 70.86,
            "temperatureMaxTime": 1566687600,
            "apparentTemperatureMin": 66.81,
            "apparentTemperatureMinTime": 1566774000,
            "apparentTemperatureMax": 70.88,
            "apparentTemperatureMaxTime": 1566687600
          },
          {
            "time": 1566774000,
            "summary": "Mostly cloudy throughout the day.",
            "icon": "cloudy",
            "sunriseTime": 1566796039,
            "sunsetTime": 1566845968,
            "moonPhase": 0.86,
            "precipIntensity": 0,
            "precipIntensityMax": 0.0004,
            "precipIntensityMaxTime": 1566777600,
            "precipProbability": 0.05,
            "temperatureHigh": 65.89,
            "temperatureHighTime": 1566799200,
            "temperatureLow": 64.47,
            "temperatureLowTime": 1566889200,
            "apparentTemperatureHigh": 66.28,
            "apparentTemperatureHighTime": 1566799200,
            "apparentTemperatureLow": 64.58,
            "apparentTemperatureLowTime": 1566889200,
            "dewPoint": 60.2,
            "humidity": 0.82,
            "pressure": 1019.73,
            "windSpeed": 12.74,
            "windGust": 23.25,
            "windGustTime": 1566835200,
            "windBearing": 271,
            "cloudCover": 0.82,
            "uvIndex": 4,
            "uvIndexTime": 1566820800,
            "visibility": 10,
            "ozone": 291.4,
            "temperatureMin": 65.33,
            "temperatureMinTime": 1566820800,
            "temperatureMax": 66.53,
            "temperatureMaxTime": 1566774000,
            "apparentTemperatureMin": 65.41,
            "apparentTemperatureMinTime": 1566856800,
            "apparentTemperatureMax": 66.81,
            "apparentTemperatureMaxTime": 1566774000
          },
          {
            "time": 1566860400,
            "summary": "Clear throughout the day.",
            "icon": "clear-day",
            "sunriseTime": 1566882528,
            "sunsetTime": 1566932245,
            "moonPhase": 0.9,
            "precipIntensity": 0.0001,
            "precipIntensityMax": 0.0003,
            "precipIntensityMaxTime": 1566946800,
            "precipProbability": 0.05,
            "precipType": "rain",
            "temperatureHigh": 65.91,
            "temperatureHighTime": 1566921600,
            "temperatureLow": 62.61,
            "temperatureLowTime": 1566975600,
            "apparentTemperatureHigh": 65.91,
            "apparentTemperatureHighTime": 1566921600,
            "apparentTemperatureLow": 62.61,
            "apparentTemperatureLowTime": 1566975600,
            "dewPoint": 58.85,
            "humidity": 0.8,
            "pressure": 1021.86,
            "windSpeed": 9.94,
            "windGust": 20.8,
            "windGustTime": 1566860400,
            "windBearing": 280,
            "cloudCover": 0.11,
            "uvIndex": 7,
            "uvIndexTime": 1566907200,
            "visibility": 10,
            "ozone": 278.8,
            "temperatureMin": 64.39,
            "temperatureMinTime": 1566903600,
            "temperatureMax": 65.91,
            "temperatureMaxTime": 1566921600,
            "apparentTemperatureMin": 64.39,
            "apparentTemperatureMinTime": 1566903600,
            "apparentTemperatureMax": 65.91,
            "apparentTemperatureMaxTime": 1566921600
          },
          {
            "time": 1566946800,
            "summary": "Clear throughout the day.",
            "icon": "clear-day",
            "sunriseTime": 1566969018,
            "sunsetTime": 1567018521,
            "moonPhase": 0.94,
            "precipIntensity": 0.0051,
            "precipIntensityMax": 0.0291,
            "precipIntensityMaxTime": 1566972000,
            "precipProbability": 0.21,
            "precipType": "rain",
            "temperatureHigh": 65.3,
            "temperatureHighTime": 1567008000,
            "temperatureLow": 62.45,
            "temperatureLowTime": 1567051200,
            "apparentTemperatureHigh": 65.3,
            "apparentTemperatureHighTime": 1567008000,
            "apparentTemperatureLow": 62.49,
            "apparentTemperatureLowTime": 1567047600,
            "dewPoint": 57.63,
            "humidity": 0.8,
            "pressure": 1019.4,
            "windSpeed": 9.12,
            "windGust": 13.12,
            "windGustTime": 1567026000,
            "windBearing": 300,
            "cloudCover": 0,
            "uvIndex": 7,
            "uvIndexTime": 1566993600,
            "visibility": 10,
            "ozone": 275.5,
            "temperatureMin": 62.46,
            "temperatureMinTime": 1566979200,
            "temperatureMax": 65.3,
            "temperatureMaxTime": 1567008000,
            "apparentTemperatureMin": 62.46,
            "apparentTemperatureMinTime": 1566979200,
            "apparentTemperatureMax": 65.3,
            "apparentTemperatureMaxTime": 1567008000
          }
        ]
      },
      "offset": 1
    };

    this.forecasts = [
      {
        temperature: res.currently.temperature,
        feelsLike: res.currently.apparentTemperature,
        precipChance: res.currently.precipProbability,
        uvIndex: res.currently.uvIndex,
        summary: res.daily.data[0].summary,
        min: res.daily.data[0].temperatureMin,
        max: res.daily.data[0].temperatureMax,
        icon: res.daily.data[0].icon
      },
      {
        min: res.daily.data[1].temperatureMin,
        max: res.daily.data[1].temperatureMax,
        icon: res.daily.data[1].icon
      },
      {
        min: res.daily.data[2].temperatureMin,
        max: res.daily.data[2].temperatureMax,
        icon: res.daily.data[2].icon
      },
      {
        min: res.daily.data[3].temperatureMin,
        max: res.daily.data[3].temperatureMax,
        icon: res.daily.data[3].icon
      }
    ];
    console.log(forecast);
    // fetch(url)
    // .then(res => res.json())
    // .then(data => console.log(data))
    // .catch(err => console.log(err))
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  background-color: royalblue;
  font-family: sans-serif;
}

main {
  width: 650px;
  height: 530px;
  margin: 100px auto;
  border-radius: 8px;
  background-color: white;
}

.info {
  height: 240px;
  border-radius: 8px 8px 0 0;
  background-color: skyblue;
}

.top {
  padding: 12px 12px 0 12px;
}

.inputbar {
  display: flex;
  height: 30px;
  width: 200px;
  background-color: rgba(0, 0, 0, .2);
  border-radius: 15px;
  align-items: center;
}

.inputbar i {
  padding: 0 12px;
  font-size: 18px;
}

input {
  font: inherit;
  height: 100%;
  width: 100%;
  background-color: transparent;
  border-radius: 0 15px 15px 0;
  border: none;
  outline: none;
}

.middle {
  font-size: 120px;
  text-align: center;
}

.bottom {
  display: flex;
  justify-content: space-around;
  text-align: center;
  padding: 6px 0;
}

.summary {
  text-align: center;
  padding-top: 12px;
}

.forecast-list {
  display: flex;
  justify-content: space-evenly;
}

.forecast {
  width: 140px;
  height: 180px;
  border-radius: 8px;
  background-color: skyblue;
}
</style>
