<template>
  <div v-if="location" class="app-content">
    <section class="info">
      <div class="top">
        <div class="location">
          <i class="fas fa-map-marker-alt"></i>
          <span>{{ location }}</span>
        </div>

        <div class="buttons">
          <button @click="toggleUnit" :class="{ active: isCelsius }">
            &deg;C
          </button>
          <button @click="toggleUnit" :class="{ active: !isCelsius }">
            &deg;F
          </button>
        </div>
      </div>

      <div class="temperature">
        {{ today.temperature | convert(isCelsius) }}&deg;
      </div>

      <div class="attributes">
        <div>
          <p>{{ today.feelsLike | convert(isCelsius) }}&deg;</p>
          <p class="name">Feels Like</p>
        </div>
        <div>
          <p>{{ today.precipProb }}%</p>
          <p class="name">Chance of Rain</p>
        </div>
        <div>
          <p>{{ today.uvIndex }}</p>
          <p class="name">UV Index</p>
        </div>
      </div>
    </section>

    <section>
      <p class="summary">{{ today.summary }}</p>
      <p class="relative-time">Updated {{ timeSince }}</p>

      <ForecastList :forecasts="forecasts" :isCelsius="isCelsius" />
    </section>

    <a href="https://darksky.net/poweredby/" class="footer">
      Powered by DarkSky
    </a>
  </div>
</template>

<script>
import ForecastList from './ForecastList.vue';

export default {
  name: 'weather-card',
  props: ['timeSince', 'weatherData'],
  components: {
    ForecastList
  },
  filters: {
    convert(value, isCelsius) {
      return isCelsius ? value : Math.round(value * 1.8 + 32);
    }
  },
  data() {
    return {
      isCelsius: true,
      today: null,
      location: null,
      forecasts: []
    };
  },
  created() {
    this.parseData();
  },
  methods: {
    toggleUnit() {
      this.isCelsius = !this.isCelsius;
    },
    parseData() {
      const data = this.weatherData;

      const icons = {
        rain: 'fa-cloud-rain',
        snow: 'fa-snowflake',
        sleet: 'fa-cloud-rain',
        wind: 'fa-wind',
        fog: 'fa-smog',
        cloudy: 'fa-cloud',
        'clear-day': 'fa-sun',
        'clear-night': 'fa-moon',
        'partly-cloudy-day': 'fa-cloud-sun',
        'partly-cloudy-night': 'fa-cloud-moon'
      };

      this.today = {
        temperature: Math.round(data.currently.temperature),
        feelsLike: Math.round(data.currently.apparentTemperature),
        precipProb: Math.round(data.currently.precipProbability),
        uvIndex: data.currently.uvIndex,
        summary: data.daily.data[0].summary
      };

      this.forecasts = [];
      for (let i = 0; i < 4; i++) {
        const current = data.daily.data[i];
        const day = new Date(current.time * 1000).toLocaleDateString('en-US', {
          weekday: 'short'
        });
        this.forecasts.push({
          day: day,
          min: Math.round(current.temperatureMin),
          max: Math.round(current.temperatureMax),
          icon: icons[current.icon]
        });
      }

      this.location = data.timezone;
    }
  }
};
</script>

<style scoped>
.app-content {
  width: 650px;
  height: 530px;
  margin: 0 auto;
  border-radius: 4px;
  background-color: white;
}

.info {
  height: 240px;
  border-radius: 4px 4px 0 0;
  background-color: var(--blue-dark);
  padding-top: 12px;
}

.top {
  padding: 0 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.location i {
  padding: 0 8px;
  font-size: 18px;
  color: var(--blue-light);
}

.buttons button {
  width: 40px;
  height: 30px;
  padding-right: 4px;
  border-radius: 4px;
  color: white;
  background-color: transparent;
}

.buttons button.active {
  background-color: var(--blue-light);
}

.temperature {
  font-size: 116px;
  text-align: center;
}

.attributes {
  display: flex;
  justify-content: space-around;
}

.name {
  color: var(--blue-lightest);
}

.summary {
  padding-top: 12px;
  color: var(--black);
}

.relative-time {
  font-size: 15px;
  color: var(--gray);
}

.footer {
  display: block;
  font-size: 15px;
  text-align: center;
  padding: 12px 0;
  color: var(--gray);
  text-decoration: none;
}
</style>