<template>
	<div id="DirectionSetter">
		<h4 @click="ToggleShowingInputFields">Set Direction</h4>
		<div v-if="showingInputFields">
			<label for="deviceVectorX">x</label>
			<input type="number" id="deviceVectorX" v-model="deviceVectorX" />

			<label for="deviceVectorY">y</label>
			<input type="number" id="deviceVectorY" v-model="deviceVectorY" />

			<label for="deviceVectorZ">z</label>
			<input type="number" id="deviceVectorZ" v-model="deviceVectorZ" />
		</div>
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "DirectionSetter",
	components: {},
	data() {
		return {
			showingInputFields: false,
			deviceVectorX: 0,
			deviceVectorY: 1,
			deviceVectorZ: 0,
		}
	},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	mounted() {
		this.EmitDeviceDirection()
	},
	methods: {
		ToggleShowingInputFields() {
			this.showingInputFields = !this.showingInputFields
		},
		EmitDeviceDirection() {
			this.$emit("set", this.deviceDirection)
		},
	},
	computed: {
		deviceDirection() {
			//normalize
			let vector = [this.deviceVectorX, this.deviceVectorY, this.deviceVectorZ]
			const norm = Math.sqrt(vector[0] ** 2 + vector[1] ** 2 + vector[2] ** 2)
			vector = vector.map((v) => v / norm)

			return vector
		},
	},
	watch: {
		deviceDirection() {
			this.EmitDeviceDirection()
		},
	},
})
</script>
