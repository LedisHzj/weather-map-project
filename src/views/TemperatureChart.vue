<template>
  <div>
    <chart class="chart" v-if="loaded" :chartdata="chartdata" :options="options"/>
  </div>
</template>

<script>
import Chart from "@/components/Chart.vue";
import axios from "axios";
export default {
  components: { Chart },
  data() {
    return {
      weatherData: null,
      endpoint: "http://localhost:8080/weather/meteo/hourly",
      lat: 40.629269,
      lng: 22.947412,
      temperature: null,
      hours: null,
      myData: null,
      chartdata: null,
      options: {
        responsive: true,
        maintainAspectRatio: false,
        title: {
          display: true,
          text: "Weather Data"
        }
      },
      loaded: false
    };
  },
  async mounted() {
    this.loaded = false
    try {
      const chartData = await axios.get(this.endpoint, {
        params: {
          apikey: "4181a631-652a-40a2-a57f-e8338074cc5a",
          lat: 40.629269,
          lon: 22.947412,
          from_date: "2021-02-26T00",
          to_date: "2021-02-26T23",
          resolution: "6km",
        },
      });
      this.temperature = Object.values(chartData.data.temperature2m.data);
      this.hours = Object.keys(chartData.data.temperature2m.data);
      this.chartdata = {
        labels: this.hours,
        datasets: [
          {
            data: this.temperature,
            label: "Temperature",
            backgroundColor: "transparent",
            borderColor: "rgba(1, 116, 188, 0.50)",
            pointBackgroundColor: "rgba(171, 71, 188, 1)"
          },
        ],
      };
      this.loaded = true
      console.log(Object.values(this.chartdata.datasets[0]));
    } catch (e) {
      console.error(e);
    }
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
