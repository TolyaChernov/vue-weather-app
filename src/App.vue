<template>
  <header class="container-fluid">
    <div class="row justify-content-center">
      <div class="bg-dark text-center weather-div col-lg-6 col-sm-10 mb-5">
        <h1>Погодное приложение</h1>
        <!-- <h2>Узнать погоду в городе {{ city }}</h2> -->
        <input
          type="text"
          class="form-control bg-none text-center mt-4"
          placeholder="Введите город"
          required
          v-model="city"
        />
        <button class="mt-5" @click="getWeather()">Узнать погоду</button>
        <p class="mt-3 error">{{ error }}</p>
        <div v-if="info != null">
          <p>{{ showTemp }}</p>
          <p>{{ showFeelsLike }}</p>
          <p>{{ showMinTemp }}</p>
          <p>{{ showMaxTemp }}</p>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import axios from 'axios'

const API_KEY = '202b1ab96a6f2b28e2f8d73f2676de84'

export default {
  computed: {
    cityName() {
      return this.city
    },
    showTemp() {
      return 'Температура: ' + this.info.main.temp
    },
    showFeelsLike() {
      return 'Температура ощущаемая: ' + this.info.main.feels_like
    },
    showMinTemp() {
      return 'Температура минимальная: ' + this.info.main.temp_min
    },
    showMaxTemp() {
      return 'Температура максимальная: ' + this.info.main.temp_max
    }
  },
  data() {
    return {
      city: '',
      error: '',
      info: null
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Нужно ввести город'
        return false
      }
      this.error = ''
      this.info = ''

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${API_KEY}`
        )
        .then((response) => (this.info = response.data))
    }
  }
}
</script>

<style lang="scss" scoped>
header {
  min-height: 100vh;
  background: rgb(231, 169, 29);
  background: linear-gradient(306deg, rgba(231, 169, 29, 1) 0%, rgba(0, 212, 255, 1) 100%);
}
.weather-div {
  color: white;
  height: 300px;
  border: 2px solid rgba(61, 236, 38, 0.2);
  border-radius: 40px;
  height: 500px;
  margin-top: 100px;
  padding: 50px;
  outline: none;
}

.bg-none {
  background: none;
  border: none;
  outline: none;
  caret-color: white;
  border-radius: 0;
  color: white;
}

input:focus {
  box-shadow: none;

  border: none;
  border-bottom: 2px solid rgba(61, 236, 38, 0.3);
  background: none;
  color: white;
}

input::placeholder {
  color: rgba(61, 236, 38, 0.3);
}

button {
  background: #e2bb44;
  color: white;
  border-radius: 10px;
  border: 2px solid orange;
  padding: 10px;
  cursor: pointer;
  margin-top: 15px;
  transition: transform 0.3s ease;
  &:hover {
    transform: scale(1.2) translateY(-5px);
  }
}

.error {
  color: rgb(247, 14, 14);
}
</style>
