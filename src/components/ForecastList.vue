<template>
  <div class="forecasts">
    <div class="card" v-for="(item, i) in forecasts" :key="i">
      <p>{{ item.day }}</p>

      <i class="fas" :class="item.icon"></i>

      <span class="range">
        <p>{{ item.min }}&deg;</p>
        <p>{{ item.max }}&deg;</p>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'forecast-list',
  props: ['forecasts']
}
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
  border: 1px solid var(--blue-lightest);
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