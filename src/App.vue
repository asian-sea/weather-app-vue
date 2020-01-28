<template>
  <div>
    <div class="citySelector">
      <p>Select a city</p>
      <select v-model="selectCity" v-on:change="select">
        <option disabled value="">Please select one</option>
        <option>Sapporo</option>
        <option>Tokyo</option>
        <option>Yokohama</option>
        <option>Nagoya</option>
        <option>Kyoto</option>
        <option>Osaka</option>
        <option>Fukuoka</option>
        <option>Naha</option>
      </select>
    </div>

    <div class="weatherContents">
      <div class="weatherInfo">
        <h1 class="city">{{selectCity}}</h1>
        <h2 class="condition">{{condition.main}}</h2>
        <h3 class="temp">{{temp | roundUp}}&deg;C</h3>
      </div>

      <div v-if="condition.icon==='01d'"><img src="./img/01d.jpg"/></div>
      <div v-else-if="condition.icon==='01n'"><img src="./img/01n.jpg"/></div>
      <div v-else-if="condition.icon==='02d'"><img src="./img/02d.jpg"/></div>
      <div v-else-if="condition.icon==='02n'"><img src="./img/02n.jpg"/></div>
      <div v-else-if="condition.icon==='03d'"><img src="./img/03d.jpg"/></div>
      <div v-else-if="condition.icon==='03n'"><img src="./img/03n.jpg"/></div>
      <div v-else-if="condition.icon==='04d'"><img src="./img/04d.jpg"/></div>
      <div v-else-if="condition.icon==='04n'"><img src="./img/04n.jpg"/></div>
      <div v-else-if="condition.icon==='09d'"><img src="./img/09d.jpg"/></div>
      <div v-else-if="condition.icon==='09n'"><img src="./img/09n.jpg"/></div>
      <div v-else-if="condition.icon==='10d'"><img src="./img/10d.jpg"/></div>
      <div v-else-if="condition.icon==='10n'"><img src="./img/10n.jpg"/></div>
      <div v-else-if="condition.icon==='11d'"><img src="./img/11d.jpg"/></div>
      <div v-else-if="condition.icon==='11n'"><img src="./img/11n.jpg"/></div>
      <div v-else-if="condition.icon==='13d'"><img src="./img/13d.jpg"/></div>
      <div v-else-if="condition.icon==='13n'"><img src="./img/13n.jpg"/></div>
      <div v-else-if="condition.icon==='50d'"><img src="./img/50d.jpg"/></div>
      <div v-else-if="condition.icon==='50n'"><img src="./img/50n.jpg"/></div>
      <div v-else-if="selectCity.length <= 0" class="noSelect">What is the Weather in your city?</div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  // import weatherpic from './components/weatherpic'
  export default {
    // components: {
      //   weatherpic
    // },
    data() {
      return{
        city: null, 
        temp: null, 
        condition: {
          main: null
        },
        selectCity: '',
      }
},
    methods:{
      select: function(){
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.selectCity},jp&units=metric&appid=552148db465b158d06ba83190efd2e0b`)
        .then(function(response){
          this.city = response.data.name
          this.temp = response.data.main.temp
          this.condition = response.data.weather[0]
        }.bind(this))
        .catch(function(error){
          // eslint-disable-next-line no-console
          console.log(error)
        })
      }
    },
    filters: {
      roundUp(value){
        return Math.ceil(value)
      }
    }

  }



</script>

<style>
body{
  margin: auto;
  color: rgb(48,68,85);
  user-select: none;
}
.citySelector{
  margin: 30px;
}
.citySelector p{
  float: left;
  margin: 0 10px 0 0;
}
select{
  cursor: pointer;
}
.city{
  height: 30px;
}
.condition{
  height: 20px;
}
.weatherContents{
  position: relative;
}
.weatherInfo{
  position: absolute;
  color: white;
  top: 50%;
  left: 50%;
  text-align: center;
  transform: translate(-50%,-50%);
}
.weatherContents img{
  width: 100%;
  height: 400px;
  object-fit: cover;
}
.noSelect{
  text-align: center;
  font-size: 30px;
  line-height: 400px;
}
</style>
