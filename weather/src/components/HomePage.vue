<script setup>
import { ref } from 'vue';

let weather = ref(null);
let locate = ref(null);
let iconImg = ref(null);
let humidity = ref(null);
let windSpeed = ref(null);
const textInput = ref('');
function saveText() {
  const enteredText = textInput.value;
  fetchData(enteredText);
}
async function fetchData(location){
  try {
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${location}&APPID=3b5abe0d2b1dc82555b9c2b91c57a4b9&units=metric`)
    const data = await response.json()
    weather.value = data.main.temp
    locate.value = data.name
    humidity.value = data.main.humidity
    windSpeed.value = data.wind.speed
    iconImg.value = `http://openweathermap.org/img/wn/${data.weather[0].icon}@4x.png`;
    console.log(data);
  } catch (error) {
    console.error('Error fetching weather:', error)
  }
}

fetchData("Toronto");

</script>

<template>
    <div class="card-wrapper">
      <div class="top">
        <img v-if="iconImg" :src="iconImg" alt="Weather Icon">
        <div class="top-right">
          <h1>{{ weather }}° C</h1>
          <h2>{{ locate }}</h2>
        </div>
      </div>
      <div class="bottom">
        <div class="bottom-left">
          <span class="humidity-icon material-symbols-outlined">humidity_percentage</span>
          <h2>{{ humidity }}% Humidity</h2>
        </div>
        <div class="bottom-right">
          <span class="wind-icon material-symbols-outlined">air</span>
          <h2>{{ windSpeed }} Wind Speed</h2>
        </div>
      </div>
      <div class="search">
          <span class="search-icon material-symbols-outlined" @click="saveText">search</span>
          <input type="text" class="search-input" placeholder="Search for a city" v-model="textInput" @keyup.enter="saveText">
      </div>
    </div>
</template>
<script>
export default {
  name: 'HomePage'
}
</script>

<style scoped>
  .card-wrapper{
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 5%;
  }

  .top{
    width: 100%;
    display: flex;
    justify-content: space-evenly;
  }

  .top-right{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .bottom{
    width: 100%;
    display: flex;
    justify-content: space-evenly;
    margin: 5% 0;
  }

  .bottom-left, .bottom-right{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
  }

  .humidity-icon, .wind-icon{
    font-size: 3em;
  }

  h1{
    font-size: 4em;
    font-weight: 800;
  }

  h2{
    font-weight: 500;
  }

  .search {
    width: 80%;
    display: flex;
    align-items: center;
    padding: 8px;
    border-radius: 28px;
    background: #f6f6f6;
    transition: box-shadow 0.25s;
  }

  .search:focus-within{
    box-shadow: 0 0 2px rgba(0, 0, 0, 0.75);
  }

  .search-input{
      font-size: 16px;
      font-family: 'Lexend', sans-serif;
      color: #333333;
      margin-left: 14px;
      outline: none;
      border: none;
      background: transparent;
  }

  .search-input::placeholder, .search-icon{
    color: rgba(0, 0, 0, 0.25)
  }

  .search-icon:hover{
    cursor: pointer;
  }

  

</style>
