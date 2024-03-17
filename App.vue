<template>
  <div class="wrapper">
    <h1>Weather</h1>
    <p>What's the weather in {{city == "" ? "your city" : cityName}}?</p>
    <input type="text" v-model="city" placeholder="City">
    <br>
    <button v-if="city != ''" @click="getWeather()">Find Info</button>
    <p v-else>Input the name of the city</p>
    <p class="error">{{ error }}</p>
    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ feelTemp }}</p>
      <p>{{ minTemp }}</p>
      <p>{{ maxTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default{
  data(){
    return{
      city:'',
      error:'',
      info:null
    }
  },
  computed:{
    cityName(){
      return "«" + this.city + "»"
    },
    showTemp(){
      return "Temperature " + this.info.main.temp
    },
    feelTemp(){
      return "Feels like " + this.info.main.feels_like
    },
    minTemp(){
      return "Min temp " + this.info.main.temp_min
    },
    maxTemp(){
      return "Max temp" + this.info.main.temp_max
    },
  },
  methods:{
    getWeather(){
      if(this.city.trim().length < 2 ){
        this.error = "Need to insert more than one symbol"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=cffc95fbe7f852008b04dcfde32cad35`)
        .then(res => (this.info = res.data))
    }
  }
}
</script>

<style scoped>
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgba(71, 184, 254, 0.183);
  padding: 20px;
  text-align: center;
  color: rgb(5, 58, 91);
  }
.wrapper h1{
  margin-top: 50px;
  }
.wrapper p{
  margin-top: 20px;
}
.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(5, 58, 91);
  color: rgb(5, 58, 91);
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus{
  border-bottom-color:rgba(0, 110, 255, 0.285); ;
}
.wrapper button{
  margin-top: 30px;
  background: rgba(5, 58, 91, 0.566);
  color:rgb(193, 231, 255);
  border-radius: 10px;
  border: 0;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover{
  transform: scale(1,1) translateY(-5px);
}
.error{
  height: 20px;
  border-radius: 50px;
  margin-top: 30px;
  color: rgba(155, 0, 0, 0.45);
}
</style>
