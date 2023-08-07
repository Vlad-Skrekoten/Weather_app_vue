<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "<" + this.city + ">"
    },

    Temp() {
      return "The Temperature: " + this.info.main.temp
    },

    feelsTemp() {
      return "Feels Temperature:" + this.info.main.feels_like
    },

    maxTemp() {
      return "Max Temperature: " + this.info.main.temp_max
    },

    minTemp() {
      return "Max Temperature: " + this.info.main.temp_min
    }


  },
  methods: {
    getWeather() {
      const symb = /[0-9]/;
      if (this.city.trim().length < 2) {
        this.error = "Error, you have to write more than 1 symbol."
        return false;
      }
      else if (symb.test(this.city)) {
        this.error = "Error, you wrote the number."
        return false;
      }

      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=bebba5b51190ab50767013cab42e0346`)
        .then(res => (this.info = res.data))

      if (this.info === undefined) {
        this.error = "Error, undefined country";
        return false;
      }

      this.error = "";
    }


  }
}
</script>

<template>
  <div class="wrapper">
    <div class="app">Weather app</div>
    <p> {{ city == "" ? "Write your city to know weather " : cityName }}</p>
    <div class="block_inputs">
      <input type="text" v-model="city" placeholder="Write your city">
      <button v-if="city != ''" @click="getWeather()">Find</button>
      <button v-else-if="city == ''">Get</button>
      <p class="error">{{ error }}</p>
    </div>

    <div class="temperature" v-if="info != null">
      <p>{{ Temp }}</p>
      <p>{{ minTemp }}</p>
      <p>{{ maxTemp }}</p>
      <p>{{ feelsTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  border-radius: 20px;
  background: #2f2b2b;
  margin: 0 5%;
  padding: 40px;
  text-align: center;
  color: #FFFF;
}

.wrapper .app {
  margin-top: 50px;
  font-size: 30px;
}

.wrapper input {
  letter-spacing: 1px;
  margin: 20px 0;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #FFFFFF;
  font-size: 14px;
  font-weight: 400;
  padding: 5px 8px;
  outline: none;
  transition: border-bottom-color .3s ease;
}

.wrapper input:focus {
  border-bottom-color: #e3bc4b;
}

.wrapper button {
  background: #e3bc4b;
  color: #FFFFFF;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  transition: transform .3s ease;
  padding: 10px 25px;
}

.wrapper button:hover {
  transform: scale(1.05) translateY(-5px);
}

.wrapper button:active {
  transform: scale(1.0) translateY(0);
}

.wrapper p {
  margin-top: 20px;
}

.wrapper temperature {
  background: #e3bc4b
}

@media (max-width: 350px) {


  .wrapper button {
    padding: 10px;
    font-size: 24px;
  }

  .wrapper button:hover {
    transform: scale(1.05);
  }

  .wrapper button:active {
    transform: scale(1.0);
  }
}

.block_inputs {
  display: grid;
  grid-template-columns: 2fr 1fr;
  justify-content: center;
  gap: 2%;
  align-items:center;
}

</style>
