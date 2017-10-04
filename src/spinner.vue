<template>

	<div class="dkf-spinner" v-show="showSpinner">

		<div class="dkf-spinner__box">

			<div class="dkf-spinner__moon" v-bind:style="spinnerStyle">

				<div class="dkf-spinner__moon__satellite" v-bind:style="[spinnerMoonStyle,animationStyle2]"></div>

				<div class="dkf-spinner__moon__ring" v-bind:style="[spinnerStyle,animationStyle3]"></div>

			</div>

		</div>

	</div>

</template>

<script lang="babel">

	const component = {
		name: 'DkfSpinner',
		template: '#dkf-spinner-template',
		props: {
			loading: {
				type: Boolean,
				default: true
			},
			minDuration: {
				type: Number,
				default: 300
			},
			color: {
				type: String,
				default: '#2d373c'
			},
			size: {
				type: String,
				default: '60px'
			}
		},
		data() {
			return {
				showSpinner: false, // should the spinner be shown
				endTime: null, // minimum time to keep the loading indicator running
				intervalFn: null, // function to be run in an interval

				// some spinner stylings
				spinnerStyle: {
					height: this.size,
					width: this.size
				}

			};
		},
		mounted() {
			this.watchLoading(this.loading);
		},
		watch: {
			// watch the loading property
			loading(newValue, oldValue) {
				this.watchLoading(newValue);
			}
		},
		methods: {
			// wath the loading property
			watchLoading(loading) {

				// only when loading is started, the end of the loading
				// is checked in an interval function
				if (loading == true) {

					// clear any previously started interval loading checks
					clearInterval(this.intervalFn);

					// store the loading state internally in the component
					this.showSpinner = true;

					// calculate the earliest end time for the loading indicator
					// note that this is to avoid very short loading spinners
					// and give the user a feeling for the loading of the data
					this.endTime = Date.now() + this.minDuration;

					// check if end time has come in intervals
					this.intervalFn = setInterval(() => {

						// we need to wait until the loading was stopped
						if (this.loading == false) {

							// calculate the diff between the end time and the current time
							let diff = this.endTime - Date.now();

							// minimum time has elapsed
							if (diff <= 0) {

								// if the countdown ends clear the interval
								clearInterval(this.intervalFn);

								// hide the spinner
								this.showSpinner = false;

								// emit event to let caller now that loading was finished
								this.$emit('loaded');
							}
						}

					}, 100);
				}
			}
		},
		computed: {

			moonSize() {
				var size = parseFloat(this.size);

				// use a different ring size for smaller elements
				if (size > 40) {
					return size / 7;
				} else {
					return size / 4;
				}
			},
			spinnerMoonStyle() {
				return {
					height: this.moonSize + 'px',
					width: this.moonSize + 'px'
				};
			},
			animationStyle2() {
				return {
					top: parseFloat(this.size) / 2 - this.moonSize / 2 + 'px',
					backgroundColor: this.color
				};
			},
			animationStyle3() {
				return {
					border: this.moonSize + 'px solid ' + this.color
				};
			}
		}
	};

	export default component;


</script>

<style lang="stylus">

	.dkf-spinner, .dkf-spinner__box,
	.dkf-spinner__moon, .dkf-spinner__moon__ring, .dkf-spinner__moon__satellite {
		box-sizing: border-box;
	}

	.dkf-spinner__moon {
		-webkit-animation: dkf-spinner-moon 0.6s 0s infinite linear;
		animation: dkf-spinner-moon 0.6s 0s infinite linear;
		-webkit-animation-fill-mode: forwards;
		animation-fill-mode: forwards;
		position: relative;
		display: block;
	}

	.dkf-spinner__moon__satellite {
		-webkit-animation: dkf-spinner-moon 0.6s 0s infinite linear;
		animation: dkf-spinner-moon 0.6s 0s linear;
		-webkit-animation-fill-mode: forwards;
		animation-fill-mode: forwards;
		opacity: 0.8;
		position: absolute;
	}

	.dkf-spinner__moon__ring {
		opacity: 0.1;
		border-radius: 100%;
	}

	@-webkit-keyframes dkf-spinner-moon {
		100% {
			-webkit-transform: rotate(360deg);
			transform: rotate(360deg);
		}
	}

	@keyframes dkf-spinner-moon {
		100% {
			-webkit-transform: rotate(360deg);
			transform: rotate(360deg);
		}
	}

</style>
