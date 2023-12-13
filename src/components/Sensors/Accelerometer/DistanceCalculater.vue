<template>
	<div id="DistanceCalculater"></div>
</template>

<script>
export default Vue.defineComponent({
	name: "DistanceCalculater",
	components: {},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	props: {
		flagMoving: {
			type: Boolean,
			default: false,
		},
		accelerationInRoom: {
			type: Array,
			default: [0, 0, 0],
		},
	},
	data() {
		return {}
	},
	methods: {},
})
</script>
