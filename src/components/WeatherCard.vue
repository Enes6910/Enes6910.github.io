<template>
  <div class="weather-card">
    <input v-model="city" @keyup.enter="fetchWeather" placeholder="Entrez une ville" class="city-input" />
    <button @click="fetchWeather" class="fetch-button">Rechercher</button>

    <div v-if="weather" class="weather-info">
      <div class="cardContainer">
        <div class="card">
          <h2 class="city">{{ weather.name }}</h2>
          <p class="weather">{{ weather.weather[0].description }}</p>
          <i :class="weatherIcon" class="weather-icon"></i>
          <p class="temp">{{ weather.main.temp }}°C</p>
          <div class="minmaxContainer">
            <div class="min">
              <p class="minHeading">Min</p>
              <p class="minTemp">{{ weather.main.temp_min }}°C</p>
            </div>
            <div class="max">
              <p class="maxHeading">Max</p>
              <p class="maxTemp">{{ weather.main.temp_max }}°C</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <p v-if="error" class="error-message">{{ error }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      weather: null,
      error: null,
    };
  },
  computed: {
    weatherIcon() {
      if (!this.weather) return '';
      const description = this.weather.weather[0].main.toLowerCase();

      // Associe les icônes météo en fonction de la description
      const icons = {
        clear: 'fas fa-sun',
        clouds: 'fas fa-cloud',
        rain: 'fas fa-cloud-showers-heavy',
        drizzle: 'fas fa-cloud-rain',
        thunderstorm: 'fas fa-bolt',
        snow: 'fas fa-snowflake',
        mist: 'fas fa-smog',
        fog: 'fas fa-smog',
      };

      // Retourne l'icône correspondante ou une icône par défaut
      return icons[description] || 'fas fa-question';
    },
  },
  methods: {
    async fetchWeather() {
      this.error = null;
      this.weather = null;

      if (!this.city) {
        this.error = "Veuillez entrer une ville.";
        return;
      }

      try {
        const apiKey = 'c8693c7ff4022503fb27dd40499f35f4'; // Remplacez par votre clé OpenWeatherMap
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric&lang=fr`
        );
        if (!response.ok) throw new Error("Ville introuvable.");
        this.weather = await response.json();
      } catch (err) {
        this.error = err.message;
      }
    },
  },
};
</script>
