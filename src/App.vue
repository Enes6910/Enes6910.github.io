<template>
  <div class="app-container">
    <h1>Météo du Jour</h1>
    <WeatherCard />
  </div>
</template>

<script>
import WeatherCard from './components/WeatherCard.vue';

export default {
  components: {
    WeatherCard
  }
};
</script>

<style scoped>
.app-container {
  text-align: center;
  font-family: Arial, sans-serif;
  padding: 20px;
}

</style>
