<template>
  <div class="container">
    <div class="form-group">
      <h3>Ülke Seç</h3>
      <select v-model="selectedCountry" @change="getDataByCountry">
        <option value="selectedCountry">Global</option>
        <option
          v-for="(country, Country) in countries"
          :key="Country"
          :value="country.Slug"
        >
          {{ country.Country }}
        </option>
      </select>
    </div>
    <div>
      <div class="row justify-content-around">

        <div class="col-md-4">
          <div class="stats-item mt-3 btn btn-info">
            <i class="fa fa-viruses"></i>

            <span class="item-title">Tehşisli</span>
            <span class="item-count">{{ confirmed | numberFormat }}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item mt-3 btn btn-danger">
            <i class="fa fa-skull"></i>

            <span class="item-title">Ölüm</span>
            <span class="item-count">{{death | numberFormat}}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item mt-3 btn btn-success">
            <i class="fa fa-head-side-mask"></i>

            <span class="item-title">İyileşme</span>
            <span class="item-count">{{recovered | numberFormat}}</span>
          </div>
        </div>
      </div>
      <hr />

      <table class="table">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Tarih</th>
            <th scope="col">Tehşisli</th>
            <th scope="col">Ölüm</th>
            <th scope="col">İyileşme</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,index) in data.Countries" :key="index">
            <th>{{index}}</th>
            <td>{{item.Country | numberFormat}}</td>
            <td>{{item.TotalConfirmed | numberFormat}}</td>
            <td>{{item.TotalDeaths | numberFormat}}</td>
            <td>{{item.TotalRecovered | numberFormat}}</td>
          </tr>
        </tbody>
      </table>
          <p>Halil Y.K</p>

    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      selectedCountry: "",
      data: {},
      countries: {},
      confirmed: 0,
      death: 0,
      recovered: 0,
    };
  },
  filters:{
    numberFormat(number){
      return number.toLocaleString();
    }
  },

  methods: {
    getCountries() {
      axios.get("https://api.covid19api.com/countries").then((response) => {
        this.countries = response.data;
        console.log(this.countries);
      });
    },

    getSummaryData() {
      axios.get("https://api.covid19api.com/summary").then((response) => { 
        const data = response.data;
        this.data = data;
        this.confirmed = data.Global.TotalConfirmed; 
        this.death = data.Global.TotalDeaths;
        this.recovered = data.Global.TotalRecovered;
      });
    },
    getDataByCountry(){
      let data = this.data.Countries.filter(country=> {
        return country.Slug == this.selectedCountry;
      });

        data = data[0]
        this.confirmed = data.TotalConfirmed; 
        this.death = data.TotalDeaths;
        this.recovered = data.TotalRecovered;

    }
  },

  mounted() {
    this.getCountries();
    this.getSummaryData();
  },
};
</script>
<style>
body {
  background: slateblue;
}
table {
  background: #9a8df2;
  color: rgb(23, 53, 123);
  border-radius: 10px;
}
.container {
  margin-top: 40px;
}
.stats-item {
  padding: 20px;
  text-align: center;
  margin-bottom: 20px;
}
.item-title {
  display: block;
  color: #fff;
  font-size: 17px;
}
.item-count {
  font-weight: bold;
  font-size: 30px;
}


.btn-info {
  width: 100%;
}

.btn-danger {
  width: 100%;
}


.btn-success {
  width: 100%;
}


.form-group > select:nth-child(1) {
  width: 100%;
}

</style>
