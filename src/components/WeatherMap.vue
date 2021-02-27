<template>
  <div>
    <v-container class="grey lighten-5 mb-6">
      <v-row>
        <v-col>
          <h1>The Weather Today</h1>
        </v-col>
      </v-row>
      <v-row align="start" gutters="2" style="height: 150px;">
        <v-col md="6" sm="12" cols="12">
          <l-map
            style="height: 650px"
            :zoom="zoom"
            :center="center"
            @click="clickOnMap"
          >
            <l-tile-layer :url="url"></l-tile-layer>
            <l-marker :lat-lng="markerLatLng">
              <l-tooltip>
                <h3>Weather Now</h3>
                <p>Temperature: {{ currentTemp }}{{ tempUnit }}</p>
                <p>Humidity: {{ currentHum }}{{ humUnit }}</p>
              </l-tooltip>
            </l-marker>
          </l-map>
        </v-col>
        <v-col md="6" sm="12" cols="12">
          <v-card v-if="weatherData" class="pa-2 ds-flex" outlined tile>
            <ul class="weather-list">
              <li
                class="list-item"
                v-for="(value, key) in temperature"
                :key="key"
              >
                <v-icon class="clock-icon" color="teal darken-2"
                  >mdi-clock</v-icon
                >
                {{ key.substring(11, 16) }} - Temprature {{ value.toFixed(1)
                }}{{ tempUnit }}
              </li>
            </ul>
            <ul class="weather-list">
              <li
                class="list-item"
                v-for="(value, key, index) in humidity"
                :key="index"
              >
                - Humidity {{ value.toFixed(1) }}{{ humUnit }}
              </li>
            </ul>
          </v-card>
          <v-card class="txt-card" v-else>
            <p>
              Click on the map below to check the temperature and the humidity
              in the area of your interest
            </p>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LTooltip } from "vue2-leaflet";
import axios from "axios";

export default {
  name: "WeatherMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LTooltip,
  },
  data() {
    return {
      url: "https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png",
      zoom: 6,
      center: [38.2749497, 23.8102717],
      markerLatLng: [40.629269, 22.947412],
      weatherData: null,
      endpoint: "http://localhost:8080/weather/meteo/hourly",
      lat: 40.629269,
      lng: 22.947412,
      temperature: null,
      humidity: null,
      tempUnit: "C",
      humUnit: "%",
      newTime: "",
      currentTemp: null,
      currentHum: null,
    };
  },
  methods: {
    getWeatherData() {
      axios
        .get(this.endpoint, {
          params: {
            apikey: "4181a631-652a-40a2-a57f-e8338074cc5a",
            lat: this.lat,
            lon: this.lng,
            from_date: "2021-02-26T00",
            to_date: "2021-02-26T23",
            resolution: "6km",
          },
        })
        .then((response) => {
          this.weatherData = response.data;
          this.temperature = this.weatherData.temperature2m.data;
          this.humidity = this.weatherData.rh2m.data;
          this.tempUnit = this.weatherData.temperature2m.unit;
          this.humUnit = this.weatherData.rh2m.unit;
        })
        .catch((error) => {
          console.log("-----error-------");
          console.log(error);
        });
    },
    clickOnMap(mapEvent) {
      const { lat, lng } = mapEvent.latlng;
      this.lat = lat;
      this.lng = lng;
      this.markerLatLng = mapEvent.latlng;

      this.dateTime = new Date();
      this.newTime = this.dateTime.toISOString().substring(0, 13);

      axios
        .get(this.endpoint, {
          params: {
            apikey: "4181a631-652a-40a2-a57f-e8338074cc5a",
            lat: this.lat,
            lon: this.lng,
            from_date: this.newTime,
            to_date: this.newTime,
            resolution: "6km",
          },
        })
        .then((response) => {
          this.weatherData = response.data;
          this.currentTemp = Object.values(
            this.weatherData.temperature2m.data
          )[0].toFixed(1);
          this.currentHum = Object.values(
            this.weatherData.rh2m.data
          )[0].toFixed(1);
        })
        .catch((error) => {
          console.log("-----error-------");
          console.log(error);
        });

      this.getWeatherData();
    },
  },
};
</script>

<style scoped>
.weather-list {
  list-style: none;
}

.list-item {
  padding: 2px 0;
  display: flex;
}

.clock-icon {
  padding: 0 10px;
}

.txt-card {
  padding: 8px;
}

.ds-flex {
  display: flex;
  justify-content: center;
}
</style>
