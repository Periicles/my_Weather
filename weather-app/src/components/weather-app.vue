<template>
    <div class="home">
      <h1>{{ msg }}</h1>
      <p>
        Fine application to keep up with weather!
      </p>
    </div>
    <div class="search-box">
      <form @submit.prevent="searchCity">
        <input type="text" v-model="cityName" placeholder="Enter a city name" />
        <button type="submit">Search</button>
      </form>
    </div>
    <!-- Weather Info Box -->
    <div class="weather-info" v-if="weatherData">
      <h2>{{ weatherData.location.name }}</h2>
      <p>{{ weatherData.location.region }}, {{ weatherData.location.country }}</p>
      <p>Temperature: {{ weatherData.current.temperature }}°C</p>
      <p>{{ weatherData.current.weather_descriptions.join(', ') }}</p>
      <img :src="weatherData.current.weather_icons[0]" alt="Weather Icon">
      <button @click="openMoreDetails" class="more-details-btn">More Details</button>
    </div>
  </template>

  <script>
  export default {
    name: 'weatherApp',
    props: {
      msg: String
    },
    data() {
      return {
        cityName: '',
        weatherData: null,
      }
    },
    methods: {
        async searchCity() {
            const apiKey = process.env.VUE_APP_WEATHER_API_KEY; // Ensure you have set this in your .env file
            const url = `http://api.weatherstack.com/current?access_key=${apiKey}&query=${encodeURIComponent(this.cityName)}`;
            try {
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error('Failed to fetch weather data');
                }
                const data = await response.json();
                if (data.error) {
                    throw new Error(data.error.info);
                }
                this.weatherData = data;
                console.log(data)
                } catch (error) {
                    console.error("Error: ", error.message);
                    this.weatherData = null;
                }
            this.cityName = '';
        },
        openMoreDetails() {
            const detailsUrl = `http://www.weatherstack.com/${this.weatherData.location.name}`;
            window.open(detailsUrl, '_blank');
        },
    }
  }
  </script>


<style scoped>
.search-box {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.search-box form {
  display: flex;
  width: 100%;
  max-width: 500px;
}

.search-box input[type="text"] {
  flex-grow: 1;
  padding: 10px;
  border: 2px solid #007BFF;
  border-right: none;
  border-radius: 5px 0 0 5px;
  outline: none;
}

.search-box button {
  padding: 10px 20px;
  border: none;
  background-color: #28a745;
  color: white;
  text-transform: uppercase;
  cursor: pointer;
  border-radius: 0 5px 5px 0;
  transition: background-color 0.3s;
}

.search-box button:hover {
  background-color: #218838;
}

.weather-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
    background-color: #007BFF;
}

.weather-info h2, .weather-info p {
    margin: 5px 0;
    color: #FFFFFF;
    text-align: center;
}

.weather-info img {
    width: 50px;
    height: auto;
    margin-bottom: 10px;
}

.more-details-btn {
    background-color: #0056b3;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    text-transform: uppercase;
    font-weight: bold;
    margin-top: 10px;
    transition: background-color 0.3s;
}

.more-details-btn:hover {
    background-color: #003580;
}
</style>
