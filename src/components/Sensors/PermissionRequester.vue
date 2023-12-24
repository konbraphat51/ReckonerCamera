<template>
	<div id="PermissionRequester">
		<button v-on:click="RequestPermission">
			{{ t("permissionRequester.request_sensor") }}
		</button>
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "PermissionRequester",
	components: {},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	methods: {
		RequestPermission() {
			if (typeof DeviceMotionEvent.requestPermission === "function") {
				DeviceMotionEvent.requestPermission()
					.then((permissionState) => {
						if (permissionState === "granted") {
							this.$emit("permissionGranted")
						}
					})
					.catch(console.error)
			} else {
				// handle regular non iOS 13+ devices
			}
		},
	},
})
</script>

<i18n>
    {
        "en": {
            "permissionRequester": {
                "request_sensor": "Get sensor permission"
            }
        },
        "ja": {
            "permissionRequester": {
                "request_sensor": "センサー許可を得る"
            }
        }
    }
</i18n>

<style>
#PermissionRequester {
	margin-top: 30px;
}

#PermissionRequester button {
	width: 20%;
	height: 100px;
	background-color: paleturquoise;
	color: black;
}
</style>
