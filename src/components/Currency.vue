<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <select name="base" v-model="base">
      <option v-bind:value="currency" v-for="currency in currency_list">
        {{ currency.name }}
      </option>
    </select>
    <select name="target" v-model="target">
      <option v-bind:value="currency" v-for="currency in currency_list">
        {{ currency.name }}
      </option>
    </select>
  </div>
</template>

<script>
import axios from 'axios';
const fixerUrl = 'http://api.fixer.io/latest'

function getRates (base) {
  return axios.get(fixerUrl, {
    params: {
      base: base
    }
  }).then(res => {

    const {data} = res;
    var currency_list = [];

    currency_list.push({
      name: data.base,
      value: 1
    });

    for(var rate in data.rates) {
      currency_list.push({
        name: rate,
        value: data.rates[rate]
      });
    }

    return currency_list
  
  })
}

export default {
  name: 'currency',
  data () {
    return {
      title: 'Currency Converter',
      currency_list: [],
      base: '',
      target: '',
      base_amount: 0,
      target_amount: 0
    }
  },
  created: function () {

    getRates('USD')
      .then(res => {
        this.currency_list = res;
        this.base = res[0];
        this.target = res[1];
      })
    
  },
  updated: function () {
    console.log(this.base)
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
,