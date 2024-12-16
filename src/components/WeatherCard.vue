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
  
  <style scoped>
  .weather-card {
    max-width: 400px;
    margin: 0 auto;
    text-align: center;
  }
  
  .city-input {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .fetch-button {
    padding: 10px 20px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .weather-info {
    margin-top: 20px;
  }
  
  .error-message {
    color: red;
    margin-top: 10px;
  }
  </style>
  