<template>
  <div id="app">
    <img src="./assets/logo.png">

    <table class="table" v-if="showGrid==1" >
      <tbody>
      <tr>
        <td>id</td>
        <td>Mail</td>
        <td>Ad</td>
        <td>Soyad</td>
        <td>Resim</td>
        <td>Detay</td>
      </tr>
      <tr v-for="(tb,index) in employeeListData" :key="tb.id">
        <td>{{tb.id}}</td> 
        <td>{{tb.email}}</td>
        <td>{{tb.first_name}}</td>
        <td>{{tb.last_name}}</td>
        <td> <img v-bind:src="tb.avatar" /> </td>
        <td><button  class="btn btn-primary" v-on:click="EmployeeDetail(tb)">Detayını Gör</button></td>
      </tr>
      </tbody>
    </table>

    <table class="table" v-if="showSingleEmployee==1"> 
        <tr>
        <td>{{employeeSingleData.id}}</td>
        <td>{{employeeSingleData.email}}</td>
        <td>{{employeeSingleData.first_name}}</td>
        <td>{{employeeSingleData.last_name}}</td>
         <td> <img v-bind:src="employeeSingleData.avatar" /> </td>
         <td><button  class="btn btn-danger" v-on:click="DetailBack()">Geri Dön</button></td>
        </tr>
    </table>
 

  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'App',
  created() {
    
  },
  mounted() {
    axios.get("https://reqres.in/api/users").then(response =>(this.employeeListData=response.data.data))
  },
  methods: {
    EmployeeDetail(element){
       this.showGrid=0;
        axios.get("https://reqres.in/api/users/"+element.id).then(response=> (this.employeeSingleData=response.data.data)).catch(
         function(error){
            console.log(error);
         }
       );
       this.showSingleEmployee=1
    },
    DetailBack(){
      this.showSingleEmployee=0;
      this.showGrid=1;
    }
  },
  computed: {
    
  },
  updated() {
     console.log(this.employeeSingleData);
  },
  data() {
    return {
       employeeListData:[],
       showGrid:1,
       showSingleEmployee:0,
       employeeSingleData:null
    }
  },
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
