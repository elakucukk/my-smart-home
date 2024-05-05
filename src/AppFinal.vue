<template>
  <div id="app">
    <h1>My Home Data</h1>
    <div v-if="weatherData.length > 0">
      <ul>
        <li v-for="(data, index) in weatherData" :key="index">
          Temperature: {{ data.temperature.toFixed(1) }}°C, Humidity: {{ data.humidity.toFixed(1) }}%
        </li>
      </ul>
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
      weatherData: []  // Initialize weatherData as an empty array
    };
  },
  methods: {
    fetchData() {
      axios.get('http://localhost:8080/allHome')
        .then(response => {
          this.weatherData = response.data;  // Assign response data to weatherData
        })
        .catch(error => {
          console.error("There was an error fetching the weather data:", error);
        });
    }
  },
  mounted() {
    this.fetchData();  // Call fetchData when component mounts
    setInterval(this.fetchData, 30000);  // Refresh data every 30 seconds
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
ul {
  list-style-type: none;
  padding: 0;
}
li {
  padding: 8px;
  margin-bottom: 6px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
}
</style>
