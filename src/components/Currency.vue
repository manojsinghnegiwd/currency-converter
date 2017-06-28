<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <symbol-list v-on:symbolChange="symbolChange" :list="baseList()" name="base" :initialvalue="base"></symbol-list>
    <symbol-list v-on:symbolChange="symbolChange" :list="targetList()" name="target" :initialvalue="target"></symbol-list>

    <input v-on:keyup="convertRates" type="number" name="base_amount" v-model="base_amount">

    <span>{{target_amount}}</span>

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

function convertRates (base, target) {
  return axios.get(fixerUrl, {
    params: {
      base: base,
      symbols: target
    }
  }).then(res => {

    const {data} = res;
    var currency_list = [];

    return data.rates[target];
  
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
      base_amount: 1,
      target_amount: 1
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
      this[name] = val;
      this.convertRates()
    },
    convertRates: function () {
      convertRates(this.base.name, this.target.name)
        .then(res => this.target_amount = res * this.base_amount);
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