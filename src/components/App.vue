<template>
	<div id="app">
		<LanguageSelection />
		<Camera @pictureTaken="OnPictureTaken" />
		<Sensors ref="sensors" />
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "App",
	components: {
		LanguageSelection: Vue.defineAsyncComponent(() =>
			loadModule("src/components/LanguageSelection.vue", options),
		),
		Camera: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Camera/Camera.vue", options),
		),
		Sensors: Vue.defineAsyncComponent(() =>
			loadModule("src/components/Sensors/Sensors.vue", options),
		),
	},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	methods: {
		OnPictureTaken(data) {
			const pictureData = data

			const position = this.$refs["sensors"].GetPosition()
			const room2device = this.$refs["sensors"].GetRoom2Device()

			const dataEdited = this.CreateImageData(
				pictureData,
				position,
				room2device,
			)

			const time = new Date().toISOString().replace(/:/g, "-")

			this.Download(dataEdited, "image_" + time + ".jpg")
		},
		CreateImageData(pictureData, position, room2device) {
			//make exif
			let exif = {}
			exif[piexif.ExifIFD.MakerNote] = this.WriteMakerNote(
				position,
				room2device,
			)
			const exifObj = {
				Exif: exif,
			}
			const exifStr = piexif.dump(exifObj)

			//write to PNG
			return piexif.insert(exifStr, pictureData)
		},
		Download(data, filename) {
			const link = document.createElement("a")
			link.href = data
			link.download = filename
			link.click()

			link.remove()
		},
		WriteMakerNote(position, room2device) {
			const makerNote = {
				direction_w: room2device.w,
				direction_x: room2device.x,
				direction_y: room2device.y,
				direction_z: room2device.z,
				position_x: position[0],
				position_y: position[1],
				position_z: position[2],
			}
			return JSON.stringify(makerNote)
		},
	},
})
</script>
