<template>
  <div></div>
</template>

<script setup>
import axios from 'axios'
import { useRoute } from 'vue-router'

const route = useRoute()
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=ba2845e7b8839a53aaf1749f8326c1fd&units=imperial`
    )

    const localOffset = new Date().getTimezoneOffset() * 600000
    const utc = weatherData.data.current.dt * 1000 + localOffset
    weatherData.data.currentTime = utc + weatherData.data.timezone_offset * 1000

    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset
      hour.currentTime = utc + weatherData.data.timezone_offset * 1000
    })

    return weatherData
  } catch (error) {
    console.log(error)
  }
}
const weatherData = await getWeatherData()
console.log(weatherData)
</script>
