<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <select name="base" v-model="base">
      <option v-for="currency in currency_list">
        {{ currency }}
      </option>
    </select>
    <select name="to" v-model="to">
      <option v-for="currency in currency_list">
        {{ currency }}
      </option>
    </select>
    <input name="base_amount" v-model="base_amount" />
    <input name="to_amount" v-model="to_amount" />
  </div>
</template>

<script>
import axios from 'axios';
const fixerUrl = 'http://api.fixer.io/latest'

var currency_list = [
  'USD',
  'INR',
  'GBP'
]

export default {
  name: 'currency',
  data () {
    return {
      title: 'Currency Converter',
      currency_list: [],
      base: '',
      to: '',
      base_amount: 0,
      to_amount: 0
    }
  },
  created: function () {

    axios.get(fixerUrl)
      .then((res) => {

        this.currency_list.push(res.data.base);

        for(var rate in res.data.rates) {
          this.currency_list.push(rate);
        }

        this.base = this.currency_list[0];
        this.to = this.currency_list[1];

      })
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
,