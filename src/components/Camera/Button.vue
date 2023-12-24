<template>
	<div id="CameraButton">
		<button @click="takePicture">{{ t("camera.takePicture") }}</button>
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "Camera",
	components: {},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	methods: {
		takePicture() {
			this.$emit("takePicture")
		},
	},
})
</script>

<i18n>
{
    "en": {
        "camera" : {
            "takePicture": "Take Picture"
        }
    },
    "ja": {
        "camera" : {
            "takePicture": "撮影"
        }
    }
}
</i18n>

<style>
#CameraButton {
	position: absolute;
	top: 0;
	right: 0;
	z-index: 100;
}

#CameraButton button {
	width: 100px;
	height: 100px;
	border-radius: 50%;
	background-color: pink;
	border: 1px solid #000;
	color: black;
}
</style>
