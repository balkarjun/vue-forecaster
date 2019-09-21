<template>
  <div v-if="location" class="app-content">
    <section class="info">
      <div class="top">
        <div class="location">
          <i class="fas fa-map-marker-alt"></i>
          <span>{{ location }}</span>
        </div>
        <div class="buttons">
          <button @click="toggleUnit" :class="{active: isCelsius}">C</button>
          <button @click="toggleUnit" :class="{active: !isCelsius}">F</button>
        </div>
      </div>
      <p class="temperature">
        {{ today.temperature }}
      </p>
      <div class="extras">
        <div>
          <p>{{ today.feelsLike }}</p>
          <p class="attribute">Feels Like</p>
        </div>
        <div>
          <p>{{ today.precipChance }}%</p>
          <p class="attribute">Chance of Rain</p>
        </div>
        <div>
          <p>{{ today.uvIndex }}</p>
          <p class="attribute">UV Index</p>
        </div>
      </div>
    </section>
    <section>
      <p class="summary">{{ today.summary }}</p>
      <p class="relative-time">Updated {{ timeSince }}</p>
      <div class="forecasts">
        <div class="card" v-for="(item, index) in forecasts" :key="index" :class="{active: index === 0}" >
          <p>{{ item.day }}</p>
          <i class="fas" :class="item.icon"></i>
          <span class="range">
            <p>{{ item.min }}</p>
            <p>{{ item.max }}</p>
          </span>
        </div>
      </div>
    </section>
    <a href="https://darksky.net/poweredby/" class="footer">
      Powered by DarkSky
    </a>
  </div>
</template>

<script>
export default {
  name: 'weather-card',
  props: ['timeSince', 'weatherData'],
  data() {
    return {
      isCelsius: true,
      today: null,
      location: null,
      forecasts: []
    }
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
        'clear-day': 'fa-sun',
        'clear-night': 'fa-moon',
        'rain': 'fa-cloud-rain',
        'snow': 'fa-snowflake',
        'sleet': 'fa-cloud-rain',
        'wind': 'fa-wind',
        'fog': 'fa-smog',
        'cloudy': 'fa-cloud',
        'partly-cloudy-day': 'fa-cloud-sun',
        'partly-cloudy-night': 'fa-cloud-moon'
      };

      this.today = {
        temperature: Math.round(data.currently.temperature),
        feelsLike: Math.round(data.currently.apparentTemperature),
        precipChance: Math.round(data.currently.precipProbability),
        uvIndex: data.currently.uvIndex,
        summary: data.daily.data[0].summary
      }

      this.forecasts = [];
      for (let i = 0; i < 4; i++) {
        const current = data.daily.data[i];      
        this.forecasts.push({
          day: new Date(current.time * 1000).toLocaleDateString('en-US', { weekday: 'short'} ),
          min: Math.round(current.temperatureMin),
          max: Math.round(current.temperatureMax),
          icon: icons[current.icon]
        });
      }

      this.location = data.timezone;
    }
  }
}
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
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 4px;
  color: white;
  background-color: transparent;
}

.buttons button.active {
  background-color: var(--blue-light);
}

.temperature {
  font-size: 116px;
}

.extras {
  display: flex;
  justify-content: space-around;
}

.attribute {
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

.forecasts {
  display: flex;
  justify-content: space-evenly;
  padding-top: 20px;
}

.card {
  width: 140px;
  height: 180px;
  padding-top: 16px;
  border-radius: 4px;
  color: var(--blue);
  border: 1px solid var(--blue-lightest);
}

.card.active {
  color: white;
  background-color: var(--blue-dark);
}

.card i {
  display: block;
  font-size: 56px;
  text-align: center;
  padding-top: 24px;
}

.range {
  padding-top: 26px;
  display: flex;
  justify-content: space-evenly;
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