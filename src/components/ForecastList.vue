<template>
  <div class="forecasts">
    <div class="card" v-for="(item, i) in forecasts" :key="i">
      <p>{{ item.day }}</p>

      <i class="fas" :class="item.icon"></i>

      <span class="range">
        <p>{{ item.min | convert(isCelsius) }}&deg;</p>
        <p>{{ item.max | convert(isCelsius) }}&deg;</p>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'forecast-list',
  props: ['forecasts', 'isCelsius'],
  filters: {
    convert(value, isCelsius) {
      return isCelsius ? value : Math.round(value * 1.8 + 32);
    }
  }
};
</script>

<style scoped>
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
  border: 1px solid var(--blue-light);
}

.card:first-of-type {
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
</style>