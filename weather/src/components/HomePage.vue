<script setup>
import { ref } from 'vue'
import { watchEffect } from 'vue'
import { format } from "date-fns";

let weather = ref(null);
let location = ref(null);
const date = format(new Date(), "yyyy-MM-dd HH:mm:ss");
watchEffect(async () => {
  try {
    const response = await fetch('https://api.openweathermap.org/data/2.5/weather?q=Belleville,Ontario,Canada&APPID=3b5abe0d2b1dc82555b9c2b91c57a4b9&units=metric')
    const data = await response.json()
    weather.value = data.main.temp
    location.value = data.name
  } catch (error) {
    console.error('Error fetching weather:', error)
  }
})

</script>

<template>
    <div class="home">
        <h1 class="green">Weather App</h1>
        <p>{{ date }}</p>
        <div>
            <p><b class="green">Currently it is:</b> {{ weather }}° C in {{ location }} </p>
        </div>
    </div>
</template>
<script>
export default {
  name: 'HomePage'
}
</script>

<style>
    .home{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin-top: 20%
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
