<script setup>
import { ref } from 'vue'
import { watchEffect } from 'vue'
import { format } from "date-fns";

let weather = ref(null);
let location = ref(null);
let iconImg = ref(null);
const date = format(new Date(), "EEEE, d MMMM yyyy");
const time = format(new Date(), "H:mm");
watchEffect(async () => {
  try {
    const response = await fetch('https://api.openweathermap.org/data/2.5/weather?q=Toronto&APPID=3b5abe0d2b1dc82555b9c2b91c57a4b9&units=metric')
    const data = await response.json()
    weather.value = data.main.temp
    location.value = data.name
    iconImg.value = `http://openweathermap.org/img/wn/${data.weather[0].icon}@4x.png`;
  } catch (error) {
    console.error('Error fetching weather:', error)
  }
})

</script>

<template>
    <div class="card-wrapper">
      <div class="top">
        <img v-if="iconImg" :src="iconImg" alt="Weather Icon">
        <div class="top-right">
          <p>{{ weather }}° C</p>
          <p>{{ location }}</p>
          <p>{{ time }}</p>
          <p>{{ date }}</p>
        </div>
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
  }

  .top{
    display: flex;
    justify-content: space-between;
  }

  img {
    /* background: linear-gradient(135deg, rgba(16, 15, 15, 0.507), rgba(255, 255, 255, 0));
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border-radius: 20px;
    border:1px solid rgba(255, 255, 255, 0.18); */
    margin-top: 2%;
    margin-left: 8%;
  }

  h1{
    font-size: 2rem;
    font-weight: 800;
  }

  b{
    font-weight: 700;       
  }
  p{
    font-size: 1.2rem;
  }
</style>
