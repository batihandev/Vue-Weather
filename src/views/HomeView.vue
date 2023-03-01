<script setup>
import axios from 'axios'
import { ref } from 'vue'
const searchQuery = ref('')
const queryTimeout = ref(0)
const searchError = ref(null)
const mapboxAPIKey =
  'pk.eyJ1IjoiYmF0aWhhbmRldiIsImEiOiJjbGVwMXh6dDIwMDk3M3ZwZnhwMHh6M2F3In0.LnSNenoEcKHXP0UM9A2ZpA'
const mapboxSearchResults = ref('')
const getSearchResults = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value.length > 0) {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        )
        mapboxSearchResults.value = result.data.features
        console.log(mapboxSearchResults.value)
      } catch (error) {
        console.log(error)
        searchError.value = true
      }
      return
    }
    mapboxSearchResults.value = ''
  }, 500)
}
</script>

<template>
  <main class="container text-white">
    <div class="relative mb-8 pt-4">
      <input
        v-model="searchQuery"
        type="text"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="w-full border-b bg-transparent py-2 px-1 focus:border-weather-secondary focus:shadow-[0px_1px_0_0_#004E71] focus:outline-none"
      />
      <ul
        v-if="mapboxSearchResults"
        class="absolute top-[66px] w-full bg-weather-secondary py-2 px-1 text-white shadow-md"
      >
        <p v-if="searchError">Sorry, something went wrong,please try again.</p>
        <p v-if="!serverError && mapboxSearchResults.length === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="cursor-pointer py-2"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>
