<template>
  <div id="app">
    <img src="./assets/logo.png">

    <table class="table" v-if="showGrid==1" >
      <tbody>
      <tr>
        <td>index</td>
        <td>Çalışan</td>
        <td>Maaş</td>
        <td>Yaş</td>
        <td>Resim</td>
        <td>Detay</td>
      </tr>
      <tr v-for="(tb,index) in employeeListData" :key="tb.id">
        <td>{{tb.id}}</td>
        <td>{{tb.employee_name}}</td>
        <td>{{tb.employee_salary}} $</td>
        <td>{{tb.employee_age}}</td>
        <td> <img v-bind:src="tb.profile_image" /> </td>
        <td><button  class="btn btn-primary" v-on:click="EmployeeDetail(tb)">Detayını Gör</button></td>
      </tr>
      </tbody>
    </table>

    <table class="table" v-if="showSingleEmployee==1"> 
        <tr>
        <td>{{employeeSingleData.id}}</td>
        <td>{{employeeSingleData.employee_name}}</td>
        <td>{{employeeSingleData.employee_salary}}</td>
        <td>{{employeeSingleData.employee_age}}</td>
         <td> <img v-bind:src="employeeSingleData.profile_image" /> </td>
        </tr>
    </table>

    <table class="table" v-else> 
        <tr>
        <td>Kayıt bulunamadı</td>
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
    axios.get("http://dummy.restapiexample.com/api/v1/employees").then(response =>(this.employeeListData=response.data.data))
  },
  methods: {
    EmployeeDetail(element){
       this.showGrid=0;
        axios.get("http://dummy.restapiexample.com/api/v1/employee/"+element.id,{headers: {
          'X-Requested-With': 'XMLHttpRequest',
                    'COntent-Type': 'application/json'
      }}).then(response=> (this.employeeSingleData=response.data)).catch(
         function(error){
            console.log(error);
         }
       );
 

       this.showSingleEmployee=1
    },
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
