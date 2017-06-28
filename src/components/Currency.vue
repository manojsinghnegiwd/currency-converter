<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <symbol-list v-on:symbolChange="symbolChange" :list="baseList()" name="base" :initialvalue="base"></symbol-list>
    <symbol-list v-on:symbolChange="symbolChange" :list="targetList()" name="target" :initialvalue="target"></symbol-list>
  </div>
</template>

<script>
import axios from 'axios';
import SymbolList from './SymbolList';
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
  components: {
    SymbolList
  },
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
  methods: {
    symbolChange: function (val, name) {
      this[name] = val 
    },
    baseList: function () {
      return this.currency_list.filter(currency => currency.name != this.target.name)
    },
    targetList: function () {
      return this.currency_list.filter(currency => currency.name != this.base.name)
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
,