<template>
  <main>
    <form @submit.prevent="fetchLocation" class="inputbar">
      <input type="text" placeholder="Enter a location" v-model="inputValue" />
      <button class="fas fa-search"></button>
    </form>

    <div v-if="error != null" class="error">{{ error }}</div>

    <div v-else-if="loading" class="loader"></div>

    <WeatherCard
      v-else-if="latlong"
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
      latlong: null,
      loading: false,
      error: null,
      inputValue: '',
      weatherData: null,
      start: 0,
      timeSince: 0
    };
  },
  methods: {
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
            this.latlong = `${data.latitude},${data.longitude}`;
            this.fetchInfo();
          })
          .catch(err => {
            this.error = 'Unable to fetch weather info';
          });
      }
      this.inputValue = '';
    },
    fetchInfo() {
      const key = '85e8f9c3f0ba466ca2512a8008d0df8e';
      const proxy = 'https://cors-anywhere.herokuapp.com/';
      const exclude = '[minutely,hourly,alerts,flags]';
      const units = 'si';
      const url = `${proxy}https://api.darksky.net/forecast/${key}/${this.latlong}?units=${units}&exclude=${exclude}`;

      fetch(url)
        .then(res => res.json())
        .then(data => {
          this.weatherData = data;

          this.start = new Date().getTime();
          this.updateTime();
          this.loading = false;
        });
    },
    updateTime() {
      const diff = (new Date().getTime() - this.start) / 1000;
      console.log('called');
      if (diff < 60) this.timeSince = 'just now';
      else if (diff < 3600) this.timeSince = `${parseInt(diff / 60)} min ago`;
      else this.timeSince = `${parseInt(diff / 3600)} hr ago`;
    }
  },
  mounted() {
    setInterval(this.updateTime, 60 * 1000);
  }
};
</script>

<style>
:root {
  --blue-dark: #152233;
  --blue: #2a4365;
  --blue-light: #4c688f;
  --blue-lightest: #dbe1e9;
  --red-primary: #d32f2f;
  --red-secondary: #f9e0e0;
  --gray: #666666;
  --black: #212121;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Rubik', sans-serif;
  color: white;
  background-color: var(--blue-light);
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

.inputbar {
  margin-top: 80px;
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
  color: var(--black);
}

.inputbar button {
  width: 100px;
  height: 50px;
  border-radius: 0 4px 4px 0;
  font-size: 18px;
  color: var(--blue);
  background-color: var(--blue-lightest);
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

.loader {
  margin: 50px auto;
  width: 36px;
  height: 36px;
  border: 4px solid transparent;
  border-top: 4px solid var(--blue-dark);
  border-radius: 50%;
  animation: spin 1s linear infinite;
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
