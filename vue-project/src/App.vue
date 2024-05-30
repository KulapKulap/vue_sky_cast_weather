//npm run dev
//npm i axios

<script>
import axios from 'axios'
export default {
  data() {
    return {
      city: "",
      error: "",
      API: '7f061594b3629ab634fd104c5c98f852',
      info: null,
    }
  },

  computed: {
    cityName() {
      return this.city;
    }
  },

  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Incorrect city name";
        return false;
      }
      this.error = "";
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.API}&units=metric`)
        .then(res => {
          this.info = res.data;
          this.error = ""; // Clear any previous error message
        })
        .catch(err => {
          if (err.response && err.response.status === 404) {
            this.error = "City not found";
            this.info = null; // Clear any previous weather information
          } else {
            this.error = "An error occurred. Please try again.";
          }
        });
    }
  }
}
</script>




<template>
  <div class="wrapper">
    <div class="wrapper-container">
      <h1>Sky Cast Weather</h1>
      <p>Check weather in {{ city === "" ? "your city" : cityName }}</p>
      <div class="wrapper-container-inner">
        <input type="text" v-model="city" placeholder="Enter city" @keydown.enter="getWeather">
        <button v-if="city !== ''" @click="getWeather">Check</button>
        <button disabled v-else>Enter city name</button>
      </div>
      <p class="error">{{ error }}</p>
      <div v-if="info !== null">
        <p>{{ `${Math.ceil(info.main.temp)}°C` }}</p>
      </div>
    </div>
  </div>
</template>



//scoped only for this component
//$event.target.value - for input data ====== v-model="city"
<style scoped>
.wrapper {
  width: 500px;
  height: 300px;
  border-radius: 50px;
  background: black;
  text-align: center;
  color: white;
  display: flex;
  justify-content: center;
}

.wrapper-container {
  margin-top: 40px;
}

.wrapper-container p{
  margin-top: 30px;
}

.wrapper-container-inner{
  margin-top: 30px;
}


.wrapper input {
  background: transparent;
  border: 0;
  border-bottom: 2px solid black;
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: aquamarine;
}

.wrapper button {
  background: orange;
  color: white;
  border-radius: 10px;
  border: 2px solid yellow;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:disabled {
  background: rgb(163, 163, 163);
  cursor: not-allowed;
  border: none;
}

.wrapper button:disabled:hover {
  transform: none;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: red;
}
</style>


