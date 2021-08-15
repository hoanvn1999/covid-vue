<template>
  <div id="world">
    <div class="row">
      <div class="col-12">
        <div class="text-center"><h2>Thống kê</h2></div>
        <hr>
      </div>
      <div class="col-md-6">
        <div class="input-group mb-2">
          <div class="input-group-prepend">
            <div class="input-group-text">
              Tìm kiếm
            </div>
          </div>
          <input class="form-control" type="text" v-model="text_search" v-on:input="search()" placeholder="Input country name">
        </div>
      </div>
      <div class="col-md-6">
        <select class="form-select" v-model="date_search" v-on:change="search()">
          <option v-bind:value="lated_date" :selected="true">{{ new Date(lated_date) }}</option>
          <option 
            v-for="index in 6" :key="index"
            v-bind:value = handleDate(index)
            >{{ new Date(handleDate(index)) }}
          </option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <table class="table table-striped">
          <thead class="bg-primary text-white">
            <tr>
              <th scope="col" class="cursor-pointer text-center" v-on:click="sort(0)">
                Quốc gia
                <div v-bind:class="{'' : check_arrange[0] === 0,
                                    'arrow-up' : check_arrange[0] === 1,  
                                    'arrow-down': check_arrange[0] === 2}">
                </div>
              </th>
              <th scope="col" class="cursor-pointer" v-on:click="sort(1)">
                Tổng số ca
                <div v-bind:class="{'' : check_arrange[1] === 0,
                                    'arrow-up' : check_arrange[1] === 2,  
                                    'arrow-down': check_arrange[1] === 1}">
                </div>
              </th>
              <th scope="col" class="cursor-pointer" v-on:click="sort(5)">
                Đang điều trị
                <div v-bind:class="{'' : check_arrange[5] === 0,
                                    'arrow-up' : check_arrange[5] === 2,  
                                    'arrow-down': check_arrange[5] === 1}">
                </div>
              </th>
              <th scope="col" class="cursor-pointer" v-on:click="sort(3)">
                Đã tử vong
                <div v-bind:class="{'' : check_arrange[3] === 0,
                                    'arrow-up' : check_arrange[3] === 2,  
                                    'arrow-down': check_arrange[3] === 1}">
                </div>
              </th>
              <th scope="col">Tỉ lệ tử vong</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="country in countries" :key="country.country">
              <th scope="row" class="text-center">{{ country.country }}</th>
              <td>{{ String(country.confirmed).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</td>
              <td>{{ String(country.confirmed - country.deaths).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</td>
              <td>{{ String(country.deaths).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</td>
              <td>{{ (country.deaths/country.confirmed*100).toFixed(2) }}%</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>    
  </div>
</template>

<script>
export default {
  name: 'World',
  props: {
    all_data: {type: Object, default: () => {}},
    lated_date: String
  },
  data() {
    return {      
      countries: this.all_data[this.lated_date],
      text_search: "",
      date_search: new Date().toISOString().slice(0, 10),
      check_arrange: [1, 0, 0, 0, 0, 0]
    }
  },
  methods: {
    search() {
      this.countries = [];
      if (this.text_search === '')
        this.countries = this.all_data[this.date_search]
      else
        for(let i=0; i<this.all_data[this.date_search].length; i++)
          if(('' + this.all_data[this.date_search][i].country).search(this.text_search) !== -1)
            this.countries.push(this.all_data[this.date_search][i]);
    },
    sort(field_index) {
      this.handleSortClick(field_index);
      this.countries.sort((a, b) => {
        switch(field_index){
          case 0:
            if (this.check_arrange[field_index] === 1) {
              if(a.country < b.country) return -1;
              if(a.country > b.country) return 1;
            }
            else {
              if(a.country > b.country) return -1;
              if(a.country < b.country) return 1;
            }
            break;
          case 1:
            if (this.check_arrange[field_index] === 1) {
              if(a.confirmed > b.confirmed) return -1;
              if(a.confirmed < b.confirmed) return 1;
            }
            else {
              if(a.confirmed < b.confirmed) return -1;
              if(a.confirmed > b.confirmed) return 1;
            }
            break;
          case 2:
            if (this.check_arrange[field_index] === 1) {
              if(a.recovered > b.recovered) return -1;
              if(a.recovered < b.recovered) return 1;
            }
            else {
              if(a.recovered < b.recovered) return -1;
              if(a.recovered > b.recovered) return 1;
            }
            break;
          case 3:
            if (this.check_arrange[field_index] === 1){
              if(a.deaths > b.deaths) return -1;
              if(a.deaths < b.deaths) return 1;
            }
            else {
              if(a.deaths < b.deaths) return -1;
              if(a.deaths > b.deaths) return 1;
            }
            break;
          default:
            if (this.check_arrange[field_index] === 1){
              if((a.confirmed - a.recovered - a.deaths) > (b.confirmed - b.recovered - b.deaths)) return -1;
              if((a.confirmed - a.recovered - a.deaths) < (b.confirmed - b.recovered - b.deaths)) return 1;
            }
            else {
              if((a.confirmed - a.recovered - a.deaths) < (b.confirmed - b.recovered - b.deaths)) return -1;
              if((a.confirmed - a.recovered - a.deaths) > (b.confirmed - b.recovered - b.deaths)) return 1;
            }
        }
        return 0;
      });
    },
    handleSortClick(field_index){
      const status = this.check_arrange[field_index];
      this.check_arrange = [0, 0, 0, 0, 0];
      if (status === 1)
        this.check_arrange[field_index] = 2
      else
        this.check_arrange[field_index] = 1
    },
    handleDate(number_of_dates){
      return new Date(new Date(this.lated_date).setDate(new Date(this.lated_date).getDate() - number_of_dates)).toISOString().slice(0, 10)
    }
  },
  watch: {
    all_data(val) {
      this.countries = val[this.lated_date]
    }
  }
}
</script>

<style scoped>
  .arrow-up {
    width: 0; 
    height: 0; 
    border-left: 8px solid transparent;
    border-right: 8px solid transparent;
    border-bottom: 8px solid white;
    display: inline-block;
  }

  .arrow-down {
    width: 0; 
    height: 0; 
    border-left: 8px solid transparent;
    border-right: 8px solid transparent;
    border-top: 8px solid white;
    display: inline-block;
  }
  h2 {
    margin: 30px 0 0 0;
  }
  hr{
    margin: 0 0 20px 0;
  }
</style>
