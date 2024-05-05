<template>
  <div id="app">
    <h1>Weather Data</h1>
    <div v-if="weatherData">
      <p>
        Temperature: {{ weatherData.temperature.toFixed(1) }}°C,
        Humidity: {{ weatherData.humidity.toFixed(1) }}%
      </p>
    </div>
    <div v-else>
      <p>No data available or still loading...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      weatherData: null
    };
  },
  methods: {
    fetchData() {
      axios.get('http://localhost:8080/home')
        .then(response => {
          console.log("Response data:", response.data); // Log the response data to console
          this.weatherData = response.data;
        })
        .catch(error => {
              console.error("There was an error fetching the weather data:", error);
              console.log(error.response); // Log the full response from the server
              console.log(error.message); // Log the textual message of the error
            });
    }
  },
  mounted() {
    this.fetchData();
    setInterval(this.fetchData, 30000);
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
