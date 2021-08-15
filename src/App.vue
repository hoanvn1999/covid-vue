<template>
  <div id="app">
    <Navigation :isActive = "isActive" v-on:handleClick = "handleActive" />
    <div class="container" v-if="isActive[0]">
      <div class="text-center">
        <h1>Số liệu thống kê dịch bệnh Covid-19 toàn cầu</h1>
      </div>

      <HomePage 
        v-bind:confirmed = confirmed 
        v-bind:recovered = recovered 
        v-bind:deaths = deaths 
      />

      <VietNam
        v-bind:vn_confirmed = vn_confirmed 
        v-bind:vn_recovered = vn_recovered 
        v-bind:vn_deaths = vn_deaths 
        v-bind:vn_administered = vn_administered 
        v-bind:vn_people_vaccinated = vn_people_vaccinated 
        v-bind:vn_people_partially_vaccinated = vn_people_partially_vaccinated
      />

      <World
        :all_data="all_data"
        v-bind:lated_date = lated_date
      />
    </div>
    <div v-if="isActive[1]">
      <Aid />
    </div>
  </div>
</template>

<script>
import HomePage from './components/HomePage.vue'
import VietNam from './components/VietNam.vue';
import World from './components/World.vue';
import Navigation from './components/Navigation.vue';
import Aid from './components/Aid.vue';

export default {
  name: 'App',
  components: {
    HomePage,
    VietNam,
    World,
    Navigation,
    Aid
  },
  data() {
    return {
      isActive: [true, false],  
      confirmed: 0,
      recovered: 0,
      deaths: 0,

      all_data: {"a" : "a"},
      lated_date: new Date().toISOString().slice(0, 10),
      
      vn_confirmed: 0,
      vn_recovered: 0,
      vn_deaths: 0,
      vn_administered: 0,
      vn_people_vaccinated: 0,
      vn_people_partially_vaccinated: 0
    }
  },
  methods: {
    handleDate(number_of_dates){
      return new Date(new Date(this.lated_date).setDate(new Date(this.lated_date).getDate() - number_of_dates)).toISOString().slice(0, 10)
    },
    handleActive(index){
      this.isActive = [false, false]
      this.isActive[index] = true
    }
  },

  mounted() {
    this.axios.get('https://covid-api.mmediagroup.fr/v1/cases').then(response => {
      let data = {};
      this.lated_date = new Date(response.data["Vietnam"]["All"]["updated"]).toISOString().slice(0, 10);
      this.date_search = this.lated_date;
      data[this.lated_date] = [];
      Object.keys(response.data).forEach(country_name => {
        if (country_name !== "Global"){
          let country = response.data[country_name]["All"];
          country.country = country_name;
          data[this.lated_date].push(country);
        }
      });
      this.all_data = data;

      this.confirmed += response.data["Global"]["All"]["confirmed"];
      this.deaths += response.data["Global"]["All"]["deaths"];
      this.recovered += response.data["Global"]["All"]["confirmed"] - response.data["Global"]["All"]["deaths"];
      this.vn_confirmed = response.data["Vietnam"]["All"]["confirmed"];
      this.vn_deaths = response.data["Vietnam"]["All"]["deaths"];
      this.vn_recovered = response.data["Vietnam"]["All"]["confirmed"] - response.data["Vietnam"]["All"]["deaths"];
    });
    this.axios.get('https://covid-api.mmediagroup.fr/v1/history?status=Confirmed').then(response => {
      for(let i=1; i<7; i++)
        this.all_data[this.handleDate(i)] = [];
      
      Object.keys(response.data).forEach(country_name => {
        if (country_name !== "Global"){
          for(let i=1; i<7; i++)
            this.all_data[this.handleDate(i)].push({
              country: country_name, 
              confirmed: response.data[country_name]["All"]["dates"][this.handleDate(i)],
              recovered: 0,
              deaths: 0
            });
        }
      });
      this.axios.get('https://covid-api.mmediagroup.fr/v1/history?status=Recovered').then(response => {
        Object.keys(response.data).forEach((country_name, index) => {
          if (country_name !== "Global"){
            for(let i=1; i<7; i++)
              (this.all_data[this.handleDate(i)])[index].recovered = response.data[country_name]["All"]["dates"][this.handleDate(i)]
          }
        });
      });
      this.axios.get('https://covid-api.mmediagroup.fr/v1/history?status=Deaths').then(response => {
        Object.keys(response.data).forEach((country_name, index) => {
          if (country_name !== "Global"){
            for(let i=1; i<7; i++)
              (this.all_data[this.handleDate(i)])[index].deaths = response.data[country_name]["All"]["dates"][this.handleDate(i)]
          }
        });
      });
    });
    this.axios.get('https://covid-api.mmediagroup.fr/v1/vaccines?country=Vietnam').then(response => {
      this.vn_administered = response.data["All"]["administered"];
      this.vn_people_vaccinated = response.data["All"]["people_vaccinated"];
      this.vn_people_partially_vaccinated = response.data["All"]["people_partially_vaccinated"];
    });
  }
}
</script>

<style>
  #app h1 {
    margin: 50px 0 0 0;
    font-weight: 600;
  }

  .cursor-pointer{
    cursor: pointer;
  }
</style>
