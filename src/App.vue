<script>
import axios from "axios";

  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null,
      }
    },
    computed: {
      cityName() {
        return "«" + this.city + "»"
      },
      showTemp() {
        return "Temperature: " + this.info.main.temp
      },
      showFeelsLike() {
        return "Feels Like: " + this.info.main.feels_like
      },
      showMinTemp() {
        return "Min. Temperature: " + this.info.main.temp_min
      },
      showMaxTemp() {
        return "Max. Temperature: " + this.info.main.temp_max
      },
    }, 
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "Oops... Try to enter more than one letter"
          return false
        }

        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=078ec07c7fa5d0c5539d6cf89994ee15`)
          .then(res => (this.info = res.data))
      }
    }
  }
</script>

<template>
  <div class="wrapper">
    <h1>The Weather App </h1>
    <p>Check the Weather in {{ city == "" ? "your city" : cityName}}</p>
    <input type="text" v-model="city" placeholder="Enter your city">
    <button v-if="city != ''" @click="getWeather()">Get result</button>
    <button disabled v-else="city != ''">Enter the name of the city</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
    
  </div>
</template>

<style scoped>

.error {
  color: rgba(58, 0, 0, 0.801);
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(120, 90, 120);
  text-align: center;
  color: snow;
}

.wrapper h1 {
  padding-top: 50px;
}

.wrapper p {
 padding-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid snow;
  color: snow;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input::placeholder {
  color: snow;
}

.wrapper input:focus {
  border-bottom-color: #eeaeca;
}

.wrapper button {
  background: #d65f93;
  color: snow;
  border-radius: 10px;
  border: 2px solid #b8c8f2;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:disabled {
  background: #864e67;
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px)
}
</style>
