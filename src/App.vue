<template>
  <main>
    <button @click="getCurrentLocation" class="geolocation">
      <i class="fas fa-location-arrow"></i>
      Use My Location
    </button>

    <form @submit.prevent="fetchLocation" class="inputbar">
      <input type="text" placeholder="Enter a location" v-model="inputValue" />
      <button class="fas fa-search"></button>
    </form>

    <div v-if="error" class="error">{{ error }}</div>

    <div v-else-if="loading" class="loading">
      <div class="spinner"></div>
      <p>Fetching Weather</p>
    </div>

    <WeatherCard
      v-else-if="weatherData"
      :timeSince="timeSince"
      :weatherData="weatherData"
    />
  </main>
</template>

<script>
import WeatherCard from './components/WeatherCard';

export default {
  name: 'app',
  components: {
    WeatherCard
  },
  data() {
    return {
      loading: false,
      error: null,
      inputValue: '',
      weatherData: null,
      timeSince: 'just now'
    };
  },
  methods: {
    getCurrentLocation() {
      if (navigator.geolocation) {
        const success = pos => this.fetchWeather(pos.coords);

        const error = () => (this.error = 'Unable to retrieve your location');

        this.loading = true;
        navigator.geolocation.getCurrentPosition(success, error);
      } else {
        this.error = 'Geolocation is not supported by your browser';
      }
    },
    fetchLocation() {
      this.loading = false;
      this.error = null;

      const loc = this.inputValue.trim();
      const proxy = 'https://cors-anywhere.herokuapp.com/';

      if (loc !== '') {
        const url = `${proxy}https://darksky.net/geo?q=${loc}`;

        this.loading = true;

        fetch(url)
          .then(res => res.json())
          .then(data => {
            if (data.error) {
              this.error = data.error;
              return;
            }
            this.fetchWeather(data);
          })
          .catch(err => {
            this.error = 'Unable to fetch weather info';
          });
      }
      this.inputValue = '';
    },
    fetchWeather(loc) {
      const API_KEY = process.env.VUE_APP_API_KEY;
      const proxy = 'https://cors-anywhere.herokuapp.com/';
      const exclude = '[minutely,hourly,alerts,flags]';
      const units = 'si';
      const url = `${proxy}https://api.darksky.net/forecast/${API_KEY}/${loc.latitude},${loc.longitude}?units=${units}&exclude=${exclude}`;

      fetch(url)
        .then(res => res.json())
        .then(data => {
          this.weatherData = data;

          if (this.interval) clearInterval(this.interval);

          const startTime = new Date().getTime();
          this.interval = setInterval(
            () => this.updateRelativeTime(startTime),
            60 * 1000
          );

          this.loading = false;
        });
    },
    updateRelativeTime(startTime) {
      const diff = (new Date().getTime() - startTime) / 1000;

      if (diff < 60) this.timeSince = 'just now';
      else if (diff < 3600) this.timeSince = `${parseInt(diff / 60)} min ago`;
      else this.timeSince = `${parseInt(diff / 3600)} hr ago`;
    }
  }
};
</script>

<style>
:root {
  --blue-dark: #152233;
  --blue: #2a4365;
  --blue-light: #dbe1e9;

  --red-primary: #d32f2f;
  --red-secondary: #f9e0e0;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Rubik', sans-serif;
  color: white;
  background-color: var(--blue);
}

p {
  text-align: center;
}

button {
  font: inherit;
  border: none;
  outline: none;
  cursor: pointer;
}

.geolocation {
  display: block;
  margin: 24px auto 16px auto;
  height: 36px;
  padding: 0 16px;
  border-radius: 4px;
  background-color: var(--blue-light);
  color: var(--blue-dark);
}

.geolocation i {
  padding-right: 8px;
  color: var(--blue);
}

.inputbar {
  margin-bottom: 16px;
  display: flex;
  justify-content: center;
}

.inputbar input {
  width: 550px;
  height: 50px;
  border: none;
  outline: none;
  border-radius: 4px 0 0 4px;
  font: inherit;
  padding-left: 24px;
}

.inputbar button {
  width: 100px;
  height: 50px;
  border-radius: 0 4px 4px 0;
  font-size: 18px;
  color: var(--blue);
  background-color: var(--blue-light);
}

.error {
  width: 550px;
  height: 36px;
  line-height: 36px;
  background-color: var(--red-secondary);
  color: var(--red-primary);
  margin: 0 auto;
  text-align: center;
  border-radius: 4px;
}

.loading {
  background-color: var(--blue-light);
  width: max-content;
  padding: 0 16px;
  margin: 0 auto;
  height: 36px;
  display: flex;
  border-radius: 4px;
  align-items: center;
  color: var(--blue-dark);
}

.spinner {
  margin-right: 10px;
  width: 18px;
  height: 18px;
  border: 2px solid transparent;
  border-top-color: var(--blue-dark);
  border-radius: 50%;
  animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
