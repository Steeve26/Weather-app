<template>
    <div class="image-blur-box" :class="weatherClass">
      <div class="inner-image-wrapper">
        <div class="image-box" :class="weatherClass">
          <input
            type="text"
            class="query"
            v-model="query"
            placeholder="Search State/City"
            autofocus
            @keyup.enter="fetchWeather"
          />
          <div class="weather-details" v-if="weather.main != undefined">
            <div class="country">
              <span>{{ weather.name }}, </span>
              <span>{{ weather.sys.country }}</span>
            </div>
            <p class="date">{{ dayName }} {{ month }} {{ year }}</p>
            <div class="temp">{{weather.main.temp}}°C</div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  import { isEmpty } from 'lodash';
export default {
  name: "App",
  data() {
    const today = new Date();
    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const options = { weekday: 'short' };
    const dayName = today.toLocaleDateString('en-US', options);
    let temp = '';

    return {
      api_key: "7a5b01598c3fbb5cdefe3e775fe1003e",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      temp,
      dayName,
      year,
      month,
    };
  },
  methods: {
    async fetchWeather() {
      const data = await fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
      );
      const results = await data.json();
      this.setResults(results);
    },
    setResults(results) {
      this.weather = results;
      this.temp = this.weather.main.temp
    },
  },
  computed: {
    weatherClass() {
      if(this.weather.main != undefined && !isEmpty(this.weather)) {
        if (this.temp < 15) {
          return this.temp < 10 ? 'winter' : 'rain';
        } 
        else if (this.temp < 30) 
          return 'sunny';

        else 
          return 'desert';
      }
      else
        return 'default'  
    }
  },
  mounted() {
    this.weatherClass;
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Bagel+Fat+One&family=Gasoek+One&family=Monoton&family=Poppins&display=swap');

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 100%;
}
.image-blur-box, .image-box {
  width: 100%;
  height: 100%;
}
.inner-image-wrapper {
  width: 100%;
  height: 100%;
  position: relative;
  backdrop-filter: blur(44px);
}
.image-box {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 35rem;
  transition-duration: .3s;
}
input {
  width: 85%;
  padding: 12px 2rem;
  border-radius: 24px;
  border: none;
  backdrop-filter: blur(10px);
  background: #e8f2fb70;
  margin-top: 5rem;
  outline: transparent;
  box-shadow: #64646f33 0px 7px 29px 0px;
  font-size: 1.1rem;
  transition-duration: .3s;
  text-transform: capitalize;
  text-shadow: 2px 2px 5px gray;
  color: white;
}
input:hover, input:focus {
  box-shadow: #32325d40 0px 13px 27px -5px, #0000004d 0px 8px 16px -8px;
}
::placeholder {
  color: #eee;
}
.weather-details {
  width: fit-content;
  margin-inline: auto;
  margin-top: 5rem;
  font-size: 1.6rem;
  padding: 1rem;
  color: white;
  text-shadow: 2px 2px 5px gray;
}
.temp {
  padding: 1.6rem;
  background: #e8f2fb70;
  border-radius: 10px;
  margin-top: 2rem;
  backdrop-filter: blur(10px);
  font-family: Monoton;
  font-size: 2.3rem;
  box-shadow: #00000066 0px 2px 4px, #0000004d 0px 7px 13px -3px, 
  #00000033 0px -3px 0px inset;
  width: fit-content;
  margin-inline: auto;
  transition-duration: .3s;
  text-shadow: 2px 2px 5px gray;
  cursor: default;
}
.temp:hover {
  color: #3affa3;
}
.rain {
  background: url('./assets/Downpour in an abandoned city.jpg') center/cover no-repeat;
}
.sunny {
  /* background: url('./assets/a realistic Tokyo cityscape(2).jpg') left/cover no-repeat; */
  /* background: url('./assets/studio ghibli.jpg') center/cover no-repeat; */
  background: url('./assets/studio ghibli(1).jpg') center/cover no-repeat;
}
.winter {
  background: url('./assets/lake-cabin-winter.jpg') center/cover no-repeat;
}
.desert {
  /* background: url('./assets/desert.jpg') center/cover no-repeat; */
  background: url('./assets/tatooine land.jpg') center/cover no-repeat;
}
.default {
  background: url('./assets/epic beech tree.jpg') center/cover no-repeat;
}
@media screen and (max-width: 900px) {
  .image-box {
    width: 100vw;
  }
}
</style>
