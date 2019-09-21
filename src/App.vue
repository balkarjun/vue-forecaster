<template>
  <main>
    <form @submit.prevent="updateLocation" class="inputbar">
      <input type="text" placeholder="Enter a location" v-model="inputValue">
      <button>
        <i class="fas fa-search"></i>
      </button>
    </form>
    <div class="app-loading" v-if="loading">
      <div class="loader"></div>
    </div>
    <div class="app-content" v-if="location && !loading">
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
  </main>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      start: 0,
      timeSince: 0,
      today: null,
      forecasts: [],
      isCelsius: true,
      location: null,
      latlong: null,
      inputValue: '',
      loading: false
    }
  },
  methods: {
    updateLocation() {
      const loc = this.inputValue.trim();
      const proxy = 'https://cors-anywhere.herokuapp.com/';

      if (loc !== '') {
        const url = `${proxy}https://darksky.net/geo?q=${loc}`;

        this.loading = true;

        fetch(url)
        .then(res => res.json())
        .then(data => {
          if (data.error) {
            console.log(data.error);
            this.loading = false;
          } else {
            this.latlong = `${data.latitude},${data.longitude}`;
            this.fetchInfo();
          }
        })
        .catch(err => {
          console.log(err);
          this.loading = false;
        })
      }
      this.inputValue = '';
    },
    toggleUnit() {
      this.isCelsius = !this.isCelsius;
    },
    updateTime() {
      const diff = (new Date().getTime() - this.start) / 1000;

      if (diff < 60) 
        this.timeSince = 'just now';
      else if (diff < 3600) 
        this.timeSince = `${parseInt(diff/60)} min ago`;
      else 
        this.timeSince = `${parseInt(diff/3600)} hr ago`;
    },
    fetchInfo() {
      const key = '85e8f9c3f0ba466ca2512a8008d0df8e';
      const proxy = 'https://cors-anywhere.herokuapp.com/';
      const exclude = '[minutely,hourly,alerts,flags]';
      const units = 'si';
      const url = `${proxy}https://api.darksky.net/forecast/${key}/${this.latlong}?units=${units}&exclude=${exclude}`;

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

      fetch(url)
      .then(res => res.json())
      .then(data => {
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

        this.start = new Date().getTime();
        this.updateTime();
        this.loading = false;
      });
    }
  },
  mounted() {
    setInterval(this.updateTime, 60*1000);
  }
}
</script>

<style>
:root {
  --blue-dark: #152233;
  --blue: #2A4365;
  --blue-light: #4C688F;
  --blue-lightest: #DBE1E9;
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

input {
  width: 550px;
  height: 50px;
  border: none;
  outline: none;
  border-radius: 4px 0 0 4px;
  font: inherit;
  padding-left: 24px;
  color: var(--black);
}

.inputbar {
  margin-top: 80px;
  margin-bottom: 16px;
  display: flex;
  justify-content: center;
}

.inputbar button {
  width: 100px;
  height: 50px;
  border-radius: 0 4px 4px 0;
  color: var(--blue);
  background-color: var(--blue-lightest);
}

.inputbar i {
  font-size: 18px;
}

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

.app-loading {
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

.loader {
  border: 4px solid transparent;
  border-top: 4px solid var(--blue-dark);
  border-radius: 50%;
  width: 36px;
  height: 36px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
