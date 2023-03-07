<template>
 <div :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : 'cold'">
  <main>
   <div class="search">
    <input type="text" v-model="query" @keypress="fetchWeather" placeholder="Search..." />
   </div>
   <div class="content" v-if="typeof weather.main !== 'undefined'">
    <div class="location">
     <div class="location__loc">{{ weather.name }}</div>
     <div class="location__date">{{ dateBuilder() }}</div>
    </div>
    <div class="weather">
     <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
     <div class="weath">{{ weather.weather[0].main }}</div>
    </div>
   </div>
  </main>
 </div>
</template>

<script>
export default {
 data() {
  return {
   api_weather: 'https://api.openweathermap.org/data/2.5/weather',
   api_key: '4e8a279d58ba6d59e0a0808b188a517d',
   query: '',
   weather: {},
  };
 },
 methods: {
  async fetchWeather(e) {
   if (e.key === 'Enter') {
    await fetch(`${this.api_weather}?q=${this.query}&appid=${this.api_key}&units=metric`)
     .then((res) => res.json())
     .then(this.setResult)
     .catch((err) => console.log(err));
   }
  },
  setResult(res) {
   console.log(res);
   this.weather = res;
  },
  dateBuilder() {
   let d = new Date();
   let months = [
    'Январь',
    'Февраль',
    'Март',
    'Апрель',
    'Май',
    'Июнь',
    'Июль',
    'Август',
    'Сентябрь',
    'Октябрь',
    'Ноябрь',
    'Декабрь',
   ];
   let days = ['Воскресенье', 'Понедельник', 'Вторник', 'Среда', 'Четверг', 'Пятница', 'Суббота'];
   let day = days[d.getDay()];
   let date = d.getDate();
   let month = months[d.getMonth()];
   let year = d.getFullYear();
   return `${day} ${date} ${month} ${year}`;
  },
 },
};
</script>

<style>
* {
 margin: 0;
 padding: 0;
 box-sizing: border-box;
}
body {
 font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
  sans-serif;
}
.cold {
 background-image: url('./assets/cold-bg.jpg');
 background-size: cover;
 background-position: bottom;
 transition: 0.4s;
}
.warm {
 background-image: url('./assets/warm-bg.jpg');
}
main {
 width: 100%;
 min-height: 100vh;
 padding: 25px;
 background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search {
 width: 100%;
 margin-bottom: 100px;
}
.search > input {
 display: block;
 width: 100%;
 padding: 15px;

 color: #313131;
 font-size: 20px;
 appearance: none;
 border: none;
 outline: none;
 background: none;
 box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.35);
 background-color: rgba(255, 255, 255, 0.7);
 border-radius: 0px 16px 0px 16px;
 transition: 0.4s;
}
.search > input:focus {
 box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
 background-color: rgba(255, 255, 255, 1);
 border-radius: 16px 0px 16px 0px;
}
.content {
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}
.location__loc {
 color: #fff;
 font-size: 32px;
 font-weight: 500;
 text-align: center;
 text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location__date {
 color: #fff;
 font-size: 20px;
 font-weight: 300;
 font-style: italic;
 text-align: center;
}
.weather {
 text-align: center;
}
.weather .temp {
 display: inline-block;
 padding: 10px 25px;
 color: #fff;
 font-size: 102px;
 font-weight: 900;
 text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
 background-color: rgba(255, 255, 255, 0.45);
 border-radius: 16px;
 margin: 30px 0px;
 box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather .weath {
 color: #fff;
 font-size: 48px;
 font-weight: 700;
 font-style: italic;
 text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
