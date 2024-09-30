
<template>
  <AddNewCountry @success="onSuccess"/>
  <br />
<!-- <br/> -->
    <div class="card flex justify-center">
      <Select v-model="selectedCountries" showClear :options="Countries" optionLabel="name" placeholder="Select a City" checkmark :highlightOnSelect="false" class="dropdown" @change="getDataByCountryID"/>
<br />
<br/>
<div class="card1" >

 <div>
  <h2>{{ getSpecificCountryData.name }}</h2>
  <img :src="getSpecificCountryData.flag" alt="image"/>
  <p>{{ getSpecificCountryData.rank }}</p>
 </div>
  <!-- <pre>
            {{ getSpecificCountryData }}
        </pre> -->
</div>


       
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from 'axios'
import Select from 'primevue/select';
import Card from 'primevue/card';
import AddNewCountry from "./AddNewCountry.vue";

// import Card from 'primevue/card';

const selectedCountries = ref();
const Countries = ref([]);
const getSpecificCountryData = ref({})

const  getCountries = async () => {
  try {
    const response = await axios.get(`http://localhost:8080/countries`);
    // return response.data;
      Countries.value = response.data
    // console.log(response.data)
  } catch (error) {
    console.error('Error fetching countries:', error.message);
    throw error;
  }
}


const getDataByCountryID = async(event) =>{
    console.log(event)
     const {id }  = event.value

     try {
    const response = await axios.get(`http://localhost:8080/country/${id}`);
    // return response.data;
    getSpecificCountryData.value = response.data
    console.log(response.data)
  } catch (error) {
    console.error('Error fetching countries:', error.message);
    throw error;
  }
}

const onSuccess = () =>{
  getCountries()
}

onMounted(() =>{
    getCountries()
})

</script>
