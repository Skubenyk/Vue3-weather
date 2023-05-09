<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,
    };
  },
  computed: {
    cityName() {
      return '"' + this.city + '"';
    },
    showСity() {
      return 'Місто ' + this.info.name;
    },
    showTemp() {
      return 'Температура: ' + this.info.main.temp + ' C';
    },
    showFeelsLike() {
      return 'Відчувається як: ' + this.info.main.feels_like + ' C';
    },
    showMinTemp() {
      return 'Мінімальна температура: ' + this.info.main.temp_min + ' C';
    },
    showMaxTemp() {
      return 'Максимальна температура: ' + this.info.main.temp_max + ' C';
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Назва повинна бути більше одного символа!';
        return false;
      }

      this.error = '';

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ffeaeae0f57f94ad25009d9d0f5dabe1`
        )

        .then((res) => (this.info = res.data))

        .catch((error) => {
          this.error =
            "Помилка отримання даних. Перевірте, будь ласка, правильність назви міста та наявність з'єднання з Інтернетом.";
        });

      this.city = '';
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <div>Дізнатися погоду y {{ city == '' ? 'вашому місті' : cityName }}</div>
    <input
      type="text"
      v-model="city"
      @keydown.enter="getWeather()"
      placeholder="Назвa міста"
    />
    <button v-if="city != ''" @click="getWeather()">Отримати погоду</button>
    <button disabled v-else>Введіть назву міста</button>
    <div class="error">{{ error }}</div>
    <div v-if="info != null">
      <div>
        <h3>{{ showСity }}</h3>
      </div>
      <div>{{ showTemp }}</div>
      <div>{{ showFeelsLike }}</div>
      <div>{{ showMinTemp }}</div>
      <div>{{ showMaxTemp }}</div>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: red;
}

.wrapper {
  background-color: darkslateblue;
  border-bottom-right-radius: 50px;
  border-top-left-radius: 50px;
  color: blanchedalmond;
  text-align: center;
  padding: 10px;
  height: 500px;
  width: 700px;
}

.wrapper h1 {
  margin-top: 40px;
}

.wrapper div {
  font-weight: 900;
  margin-top: 20px;
  font-size: 16px;
}

.wrapper input {
  border-bottom: 2px solid #110813;
  background: transparent;
  margin-top: 50px;
  color: #fcfcfc;
  padding: 5px 8px;
  font-size: 14px;
  outline: none;
  border: 0;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  transition: transform 500ms ease;
  border: 2px solid #b99935;
  background: #e3bc4b;
  border-radius: 10px;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  color: white;
}

.wrapper button:hover {
  transform: scale(1.07) translateY(-3px);
}
</style>
