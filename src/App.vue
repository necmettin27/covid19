<template>
  <div class="container mt-3">
     <div class="form-group">
        <select v-model="selectedCountry" class="form-control" @change="getDataByCountry">
          <option value="">Global</option>
          <option v-for="country in countries" :key="country.Slug" :value="country.Slug">{{country.Country}}</option>
        </select>
      </div>
    <div class="row justify-content-around">
     
      <div class="col-4">
        <div class="stats-item btn-info">
          <span class="item-title">Confirmed</span>
          <span class="item-count">{{confirmed | numberFormat}}</span>
        </div>
      </div>     
      <div class="col-4">
        <div class="stats-item btn-danger">
          <span class="item-title">Deaths</span>
          <span class="item-count">{{deaths | numberFormat}}</span>
        </div>
      </div>     
      <div class="col-4">
        <div class="stats-item btn-success">
          <span class="item-title">Recovered</span>
          <span class="item-count">{{recovered | numberFormat}}</span>
        </div>
      </div>
     


    </div>
    <hr/>

     <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Country</th>
              <th scope="col">Confirmed</th>
              <th scope="col">Deaths</th>
              <th scope="col">Recovered</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item,index) in data.Countries" :key="index">
              <th>{{index}}</th>
              <th>{{item.Country}}</th>
              <th>{{item.TotalConfirmed | numberFormat}}</th>
              <th>{{item.TotalDeaths | numberFormat}}</th>
              <th>{{item.TotalRecovered | numberFormat}}</th>
            </tr> 
          </tbody>
        </table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
   data(){
     return {
       countries : {},
       selectedCountry : '',
       confirmed : 0,
       deaths : 0,
       recovered : 0,
       data:{}
     }
   },
   filters:{
     numberFormat(number){
       return number.toLocaleString();
     }
   },
   methods:{
     getCountries() {
       axios.get("https://api.covid19api.com/countries")
       .then(response=>{
         this.countries = response.data;
       })
     },
     getSummaryData(){
       axios.get("https://api.covid19api.com/summary")
       .then(response=>{
         
          this.data = response.data;
          this.confirmed = response.data.Global.TotalConfirmed
          this.deaths = response.data.Global.TotalDeaths
          this.recovered = response.data.Global.TotalRecovered
       })
     },
     getDataByCountry(){
       const dat2 = this.data.Countries.filter(country => {
         return country.Slug == this.selectedCountry
       });
        const data = dat2[0];
          this.confirmed = data.TotalConfirmed
          this.deaths = data.TotalDeaths
          this.recovered = data.TotalRecovered
     }
   },
   mounted(){
    this.getCountries()
    this.getSummaryData()
   }
   
}
</script>

<style>
body{
  background: antiquewhite;
}
table{
  background:#fff;
}
.container{margin-top: 40px;}
.stats-item{
  padding: 20px;
  text-align: center;
  margin-bottom: 20px;
  margin-top: 20px;
}
.item-title{
  display: block;
  color:#fff;
  font-size: 17px;
}
.item-count{
  font-weight: bold;
  font-size: 30px;
}
</style>
