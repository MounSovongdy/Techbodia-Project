<script setup>
import { ref, watchEffect } from 'vue'

const API_URL = `https://restcountries.com/v3.1/`;
const paginations = ['all', '50'];

const currentPage = ref(paginations[0]);
const countries = ref(null);

watchEffect(async () => {
  // this effect will run immediately and then
  // re-run whenever currentBranch.value changes
  const url = `${API_URL}${currentPage.value}`
  countries.value = await (await fetch(url)).json()
})

</script>

<template>
  <template v-for="page in paginations">
    <input type="radio"
      :id="page"
      :value="page"
      name="page"
      v-model="currentPage">
    <label :for="page">{{ page }}</label>
  </template>
  <table>
    <thead>
      <tr>
        <th>Flags</th>
        <th>Country</th>
        <th>Country Code(2)</th>
        <th>Country Code(3)</th>
        <th>Native Country Name</th>
        <th>Alternative Country Name</th>
        <th>Country Calling Codes</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="country in countries">
        <th><img :src=country.flags.png style=" height: 20px;"></th>
        <td>{{country.name.official}}</td>
        <td>{{country.cca2}}</td>
        <td>{{country.cca3}}</td>
        <td>{{country.name.nativeName}}</td>
        <td>{{country.altSpellings}}</td>
        <td>{{country.idd}}</td>
      </tr>
    </tbody>
  </table>
</template>
