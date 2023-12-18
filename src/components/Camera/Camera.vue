<template>
	<div id="Camera">
		<video ref="video" autoplay playsinline width="300" height="200"></video>
		<Button @takePicture="takePicture" />
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "Camera",
	components: {
		Button: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Camera/Button.vue", options),
		),
	},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	mounted() {
		const video = this.$refs.video
		if (navigator.mediaDevices.getUserMedia) {
			navigator.mediaDevices
				.getUserMedia({video: {facingMode: "environment"}, audio: false})
				.then(function (stream) {
					video.srcObject = stream
				})
				.catch(function (error) {
					alert(error)
				})
		}
	},
	methods: {
		takePicture() {
			const canvas = document.createElement("canvas")
			const video = this.$refs.video
			canvas.width = video.videoWidth
			canvas.height = video.videoHeight
			const context = canvas.getContext("2d")
			context.drawImage(video, 0, 0, video.videoWidth, video.videoHeight)

			let data = canvas.toDataURL("image/jpeg", 1.0)

			canvas.remove()

			this.$emit("pictureTaken", data)
		},
	},
})
</script>
