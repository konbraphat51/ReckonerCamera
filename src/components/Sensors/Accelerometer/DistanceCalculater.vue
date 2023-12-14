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
		return {
			accelerationInRoomData: {
				x: [],
				y: [],
				z: [],
			},
			dataReceivedTime: [],
		}
	},
	methods: {
		StartMoving() {},
		StopMoving() {
			this.ClearData()
		},
		ClearData() {
			this.accelerationInRoomData.x = []
			this.accelerationInRoomData.y = []
			this.accelerationInRoomData.z = []
			this.dataReceivedTime = []
		},
	},
	watch: {
		accelerationInRoom: {
			handler: function (val) {
				if (!this.flagMoving) return

				this.accelerationInRoomData.x.push(val[0])
				this.accelerationInRoomData.y.push(val[1])
				this.accelerationInRoomData.z.push(val[2])
				this.dataReceivedTime.push(performance.now())
			},
			deep: true,
		},
		flagMoving: {
			handler: function (val) {
				if (val) {
					this.StartMoving()
				} else {
					this.StopMoving()
				}
			},
			deep: true,
		},
	},
})
</script>
