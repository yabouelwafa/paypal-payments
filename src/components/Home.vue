<template>
	<!-- ******************************************** Paypal payment buttons -->
	<div id="paypal-button-container"></div>
</template>

<script>
import {loadScript} from "@paypal/paypal-js"

// Prod
const PAYPAL_CLIENT_ID = "<Your PayPal Client ID>"

export default {
	components: {},

	data: () => ({
		paymentSuccessful: false,
		showPaymentSuccessfulDialog: false,
	}),

	mounted() {
		// Load Paypal SDK script
		loadScript({"client-id": PAYPAL_CLIENT_ID}).then((paypal) => {
			paypal
				.Buttons({
					// Styles
					style: {
						layout: "vertical",
						color: "blue",
						shape: "pill",
						label: "buynow",
						disableMaxWidth: true,
					},

					// Actions
					createOrder: this.createOrder,
					onApprove: this.onPaymentApprove,
					onCancel: this.onPaymentCancel,
					onError: this.onPaymentError,
				})
				.render("#paypal-button-container")
		})


	},

	methods: {
		/**
		 * Create a purchase order
		 *
		 * @param {*} data Order data
		 * @param {*} actions Paypal actions
		 */
		createOrder(data, actions) {
			return actions.order.create({
				intent: "CAPTURE",
				purchase_units: [
					{
						amount: {
							value: 10,
						},
					},
				],
			})
		},

		/**
		 * Handle approved payment
		 *
		 * @param {*} data Order data
		 * @param {*} actions Paypal actions
		 */
		onPaymentApprove(data, actions) {
			return actions.order.capture().then((response) => {
				// Payment successful

				// Show success message to the user and send the confirmation email
				this.processPO(response)
			})
		},

		/**
		 * Process the PO
		 *
		 * @param {*} paymentDetails Paypal payment transaction results
		 */
		processPO(paymentDetails) {
			// Process the PO
		},

		/**
		 * Handle payment cancellation
		 *
		 * @param {*} data
		 * @param {*} actions
		 */
		onPaymentCancel(data) {
			// Handle payment cancellation
		},

		/**
		 * Handle payment error
		 *
		 * @param {*} data
		 * @param {*} actions
		 */
		onPaymentError(data) {
			// console.log("Payment error - Data: ", data)
		},
	},
}
</script>