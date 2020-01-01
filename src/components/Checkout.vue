<template>
	<form v-on:submit.prevent="onSubmit" class="container checkout" ref="form">
		<h1 class="title">Checkout</h1>
		<div class="field">
			<label for="first-name" class="label">First name</label>
			<input
				id="first-name"
				class="input"
				type="text"
				data-recurly="first_name"
			/>
		</div>
		<div class="field">
			<label for="last-name" class="label">Last name</label>
			<input
				id="last-name"
				class="input"
				type="text"
				data-recurly="last_name"
			/>
		</div>
		<div class="field">
			<label for="recurly-hosted-field-input" class="label">Card</label>
			<div class="recurly-card" ref="recurly-card" data-recurly="card" />
		</div>
		<div class="control">
			<button class="button is-primary">Submit</button>
		</div>
	</form>
</template>

<script>
import recurly from 'recurly.js';

export default {
	name: 'Checkout',
	mounted: function() {
		recurly.configure(process.env.VUE_APP_RECURLY_KEY);
		const elements = recurly.Elements();
		const card = elements.CardElement(this.$refs['recurly-card']);
	},
	methods: {
		onSubmit(e) {
			recurly.token(this.$refs['form'], (err, token) => {
				if (err) {
					throw err;
				}

				alert(`Token: ${token.id}`);
			});
		}
	}
};
</script>

<style>
.checkout {
	margin-top: 30px;
}
.recurly-hosted-field-card {
	height: 40px;
}
</style>
