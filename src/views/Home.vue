<template>
  <div class="home">
    <WeatherMap />
  </div>
</template>

<script>
// @ is an alias to /src
import WeatherMap from '@/components/WeatherMap.vue'

export default {
  name: 'Home',
  components: {
    WeatherMap
  }
}
</script>
