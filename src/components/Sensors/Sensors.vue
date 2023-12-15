<template>
	<div id="Sensors">
		<Accelerometer
			ref="accelerometer"
			:flagMoving="flagMoving"
			@distanceCalculated="OnDistanceCalculated"
		/>
		<PermissionRequester />
		<MovingButton @startMovement="StartMovement" @stopMovement="StopMovement" />

		<Position ref="position" />
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "Sensors",
	components: {
		Accelerometer: Vue.defineAsyncComponent(() =>
			loadModule(
				"src/components/Sensors/Accelerometer/Accelerometer.vue",
				options,
			),
		),
		PermissionRequester: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Sensors/PermissionRequester.vue", options),
		),
		MovingButton: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Sensors/MovingButton.vue", options),
		),
		Position: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Sensors/Position.vue", options),
		),
	},
	data() {
		return {
			flagMoving: false,
		}
	},
	methods: {
		StartMovement() {
			this.flagMoving = true
		},
		StopMovement() {
			this.flagMoving = false
		},
		OnDistanceCalculated(distance) {
			this.$refs["position"].Move(distance)
		},
		GetPosition() {
			return this.$refs["position"].GetPosition()
		},
		GetRoom2Device() {
			let device2room = this.$refs["accelerometer"].GetDevice2RoomQuaternion()
			return Quaternion.Inverse(device2room).normalized
		},
	},
})
</script>
