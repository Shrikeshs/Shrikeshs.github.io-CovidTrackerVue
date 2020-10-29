<template>
  <div id="app" class="container">
    <div class="row my-5">
      <div class="col text-center">
        <h1>Covid 19 Data Visualization- India
          </h1>
        </div>
    </div>
 

    <div class="row mt-5" v-if="arrpositive.length > 0">
      <div class="col">
        <h2>Confirmed cases</h2>
        <LineChart
          :chartData="arrpositive"
          :options="chartOptions"
          label="Confirmed"
          :chartColors="positiveChartColors"
        >
        </LineChart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrpositive.length > 0">
      <div class="col">
        <h2>Active cases</h2>
        <LineChart
          :chartData="arrhospital"
          :options="chartOptions"
          label="Active"
           :chartColors="hospitalizedChartColors"
        >
        </LineChart>
      </div>
    </div>
    
    <div class="row mt-5" v-if="arrpositive.length > 0">
      <div class="col">
        <h2>Recovered</h2>
        <LineChart
          :chartData="arrrecovered"
          :options="chartOptions"
          label="Recovered"
           :chartColors="inIcuColors"
        >
        </LineChart>
      </div>
    </div>
  <div class="row mt-5" v-if="arrpositive.length > 0">
      <div class="col">
        <h2>Death</h2>
        <LineChart
          :chartData="arrdeath"
          :options="chartOptions"
          label="Death"
           :chartColors="deathColors"
        >
        </LineChart>
      </div>
    </div>



  </div>
</template>

<script>
 import axios from "axios";
 
import moment from "moment";
import LineChart from "./components/LineChart";
export default {
  name: "App",
  components: {
    LineChart,
  },
  data() {
    return {
      arrpositive: [],
       positiveChartColors: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#74A57F"
      },
      arrhospital: [],
      hospitalizedChartColors: {
        borderColor: "#251F47",
        pointBorderColor: "#260F26",
        pointBackgroundColor: "#858EAB",
        backgroundColor: "#858EAB"
      },
      arricu: [],
          inIcuColors: {
        borderColor: "#190B28",
        pointBorderColor: "#190B28",
        pointBackgroundColor: "#E55381",
        backgroundColor: "#E55381"
      },
      arronVenti: [],
      arrrecovered: [],
      arrdeath: [],
         deathColors: {
        borderColor: "#E06D06",
        pointBorderColor: "#E06D06",
        pointBackgroundColor: "#402A2C",
        backgroundColor: "#402A2C"
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  async created() {
    const { data } = await axios.get(" https://api.covid19api.com/total/country/india");
    
        data.forEach((d) => {
          
     const date = moment(d.Date, "YYYYMMDD").format("MM/DD");
     console.log(date);
      
      const {
       Confirmed,
        Active,
        Recovered,
        Deaths,
      } = d;
      this.arrpositive.push({ date, total: Confirmed });
      this.arrhospital.push({ date, total: Active });
      this.arrrecovered.push({ date, total: Recovered });
      this.arrdeath.push({ date, total: Deaths });
      console.log(this.arrpositive); 
    });
  },
};
</script>

<style scoped>

</style>
