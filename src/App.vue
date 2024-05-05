<template>
  <div id="app">
    <h1>My Home Data</h1>
    <div v-if="weatherData.length > 0">
      <ul class="scrollable-list">
        <li v-for="(data, index) in reversedWeatherData" :key="index">
          <!-- Display date directly from backend data -->
          {{ data.date }} - Temperature: {{ data.temperature.toFixed(1) }}°C, Humidity: {{ data.humidity.toFixed(1) }}%
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No data available...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      weatherData: []  // Init weatherData
    };
  },
  computed: {
    reversedWeatherData() {
      // Reverse the array
      return [...this.weatherData].reverse();
    }
  },
  methods: {
    fetchData() {
      axios.get('http://localhost:8080/allHome')
        .then(response => {
          this.weatherData = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the weather data:", error);
        });
    }
  },
  mounted() {
    this.fetchData();
    setInterval(this.fetchData, 1000);
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
ul.scrollable-list {
  list-style-type: none;
  padding: 0;
  max-height: 400px;
  overflow-y: auto;
  border: 1px solid #ddd;
}
li {
  padding: 8px;
  margin-bottom: 6px;
  background-color: #f9f9f9;
}
</style>
