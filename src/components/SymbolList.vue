<template>
	<div>
		<select :name="name" v-model="value">
			<option v-bind:value="currency" v-for="currency in list">
		     {{ currency.name }}
		    </option>
		</select>
	</div>
</template>

<script type="text/javascript">
export default {
	name: 'symbol-list',
	data () {
		return {
			value: {}
		}
	},
	watch: {
		initialvalue: function (val, oldVal) {
			this.value = val;
		},
		value: function (val, oldVal) {
			this.symbolChange(val)
		}
	},
	methods: {
		symbolChange: function (val) {
			this.$emit('symbolChange', val, this.name)
		}
	},
	props: ['list', 'initialvalue', 'name']
}
</script>