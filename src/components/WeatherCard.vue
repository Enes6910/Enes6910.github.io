<template>
    <div class="weather-card">
      <input 
        v-model="city" 
        @keyup.enter="fetchWeather" 
        placeholder="Entrez une ville"
        class="city-input"
      />
      <button @click="fetchWeather" class="fetch-button">Rechercher</button>
  
      <div v-if="weather" class="weather-info">
        <i :class="weatherIcon" class="weather-icon"></i>
        <h2>{{ weather.name }}</h2>
        <p>{{ weather.weather[0].description }}</p>
        <p><strong>Température :</strong> {{ weather.main.temp }}°C</p>
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
