<template>
  <div class="row" id="app">
  	<div class="one-third column">
    </div>
    <div class="one-third column">
    	<currency></currency>
    </div>
    <div class="one-third column">
    </div>
  </div>
</template>

<script>
import Currency from './components/Currency'

export default {
  name: 'app',
  components: {
    Currency
  }
}
</script>

<style>

</style>
