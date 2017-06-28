<template>
  <div class="hello">
    <h3 class="u-text-center main-heading">{{ title }}</h3>

    <div class="row">
      <div class="one-half column">
        <label>From</label>
        <symbol-list v-on:symbolChange="symbolChange" :list="baseList()" name="base" :initialvalue="base"></symbol-list>
      </div>
      <div class="one-half column">
        <label>To</label>
        <symbol-list v-on:symbolChange="symbolChange" :list="targetList()" name="target" :initialvalue="target"></symbol-list>
      </div>
    </div>
    <div class="row">
      <div>
        <label>Amount</label>
        <input min="1" v-on:change="convertRates" v-on:keyup="convertRates" type="number" name="base_amount" v-model="base_amount">
      </div>
      <div>
        <h5 class="target-amount u-text-center">{{base_amount}} {{base.name}} = {{target_amount}} {{target.name}}</h5>
        <p class="u-text-center">Made by <a href="http://manojsinghnegi.com/">Manoj Singh Negi</a> with <a href="https://vuejs.org/">Vue.js</a></p>
      </div>
    </div>
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
      title: 'Convert Currency',
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

      if(!this.base_amount) {
        this.base_amount = 1;
      }

      convertRates(this.base.name, this.target.name)
        .then(res => this.target_amount = res * parseInt(this.base_amount, 10));
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