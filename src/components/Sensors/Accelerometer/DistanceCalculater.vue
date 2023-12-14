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
			this.SaveData()

			const samplingRate = this.ComputeSamplingRate()
			const accelerationFiltered = this.FilterData(samplingRate)
			const velocityData = this.ComputeVelocity(
				accelerationFiltered,
				samplingRate,
			)
			const distance = this.ComputeDistance(velocityData, samplingRate)
			this.ClearData()
			this.$emit("distanceCalculated", distance)
		},
		FilterData(samplingRate) {
			//https://www.utsbox.com/?page_id=523
			//low pass filter

			const n = this.accelerationInRoomData.x.length

			//set
			const q = 0.7
			const cutoff = 0.01

			const omega = (2 * Math.PI * cutoff) / samplingRate
			const alpha = Math.sin(omega) / (2 * q)

			let a0 = 1 + alpha
			let a1 = -2 * Math.cos(omega)
			let a2 = 1 - alpha
			let b0 = (1 - Math.cos(omega)) / 2
			let b1 = 1 - Math.cos(omega)
			let b2 = (1 - Math.cos(omega)) / 2

			let in1 = [0, 0, 0]
			let in2 = [0, 0, 0]
			let out1 = [0, 0, 0]
			let out2 = [0, 0, 0]

			let output = []

			for (let cnt = 0; cnt < n; cnt++) {
				const input = [
					this.accelerationInRoomData.x[cnt],
					this.accelerationInRoomData.y[cnt],
					this.accelerationInRoomData.z[cnt],
				]

				output.push(input)
			}

			return output
		},
		ComputeSamplingRate() {
			//simply from average time interval
			let sum = 0
			let n = 0
			for (let i = 0; i < this.dataReceivedTime.length - 1; i++) {
				sum += this.dataReceivedTime[i + 1] - this.dataReceivedTime[i]
				n++
			}

			const miliseconds = (n - 1) / sum

			return miliseconds / 1000 //seconds
		},
		ComputeVelocity(accelerationData, samplingRate) {
			const n = accelerationData.length

			let output = [[0, 0, 0]]

			for (let cnt = 0; cnt < n; cnt++) {
				const input = [
					accelerationData[cnt][0],
					accelerationData[cnt][1],
					accelerationData[cnt][2],
				]

				const outputThis = this.PlusVec(
					this.ScaleVec(input, 1 / samplingRate),
					output[cnt],
				)

				output.push(outputThis)
			}

			return output
		},
		ComputeDistance(velocityData, samplingRate) {
			const n = velocityData.length

			let output = [0, 0, 0]

			for (let cnt = 0; cnt < n; cnt++) {
				output = this.PlusVec(
					output,
					this.ScaleVec(velocityData[cnt], 1 / samplingRate),
				)
			}

			return output
		},
		ClearData() {
			this.accelerationInRoomData.x = []
			this.accelerationInRoomData.y = []
			this.accelerationInRoomData.z = []
			this.dataReceivedTime = []
		},
		PlusVec(...vecs) {
			let result = [0, 0, 0]
			for (let vec of vecs) {
				result[0] += vec[0]
				result[1] += vec[1]
				result[2] += vec[2]
			}
			return result
		},
		ScaleVec(vec, scale) {
			return [vec[0] * scale, vec[1] * scale, vec[2] * scale]
		},
		SaveData() {
			//for debug

			let output = ""
			for (let i = 0; i < this.dataReceivedTime.length; i++) {
				let line = ""
				line += this.dataReceivedTime[i].toString() + ","
				line += this.accelerationInRoomData.x[i].toString() + ","
				line += this.accelerationInRoomData.y[i].toString() + ","
				line += this.accelerationInRoomData.z[i].toString() + "\n"
				output += line
			}

			const blob = new Blob([output], {type: "text/plain"})
			const link = document.createElement("a")
			link.href = URL.createObjectURL(blob)
			link.download = "data.csv"
			link.click()
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
