<template>
<table>
	<tr>
		<th>Vin</th>
		<th>Vout</th>
		<th>Duty Cycle</th>
	</tr>

	<tr v-if="currentData.ADC && currentData.PWM">
		<td>{{ currentData.ADC.vin.toPrecision(5) }}</td>
		<td>{{ currentData.ADC.vout.toPrecision(5) }}</td>
		<td>{{ currentData.PWM.dutyCycle }}%</td>
	</tr>

</table>
</template>


<script type="ts">
import Vue from 'vue';
import gql from 'graphql-tag';

export default Vue.extend({
	props: {
		path: {
			type: String,
			required: true,
		},
	},
	data() {
		return {
			currentData: {},
		};
	},
	apollo: {
		currentData: {
			query: gql`query currentData($path: String!) {
				currentData(path: $path) {ADC {vin, vout}, PWM {dutyCycle}}
			}`,
			variables() {
					return {
						path: this.$props.path,
					};
			},
			pollInterval: 250,
		},
	},
});
</script>

<style>
table {
	width: 100%;
	background: var(--dark);
}
th {
	font-size: var(--xlg);
	padding: var(--sm);
	color: var(--color-2-0);
}
td {
	padding: var(--md);
	font-size: var(--xlg);
	color: var(--color-1-0);
	max-width: 200px;
}

@media screen and (max-width:425px) {
	th {
		font-size: var(--sm);
		padding: var(--xsm);
	}
	td {
		font-size: var(--sm);
		padding: var(--xsm);
	}
}
</style>