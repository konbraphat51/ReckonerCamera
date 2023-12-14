<template>
	<div id="MovingButton">
		<button @click="ToggleMovement">
			{{ isMoving ? t("MovingButton.Stop") : t("MovingButton.Move") }}
		</button>
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "MovingButton",
	components: {},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	data() {
		return {
			isMoving: false,
		}
	},
	methods: {
		ToggleMovement() {
			if (this.isMoving) {
				this.$emit("stopMovement")
			} else {
				this.$emit("startMovement")
			}

			this.isMoving = !this.isMoving
		},
	},
})
</script>

<i18n>
{
    "en": {
        "MovingButton": {
            "Move": "Start Moving",
            "Stop": "Stop Moving"
        }
    },
    "ja": {
        "MovingButton": {
            "Move": "移動開始",
            "Stop": "移動停止"
        }
    }
}
</i18n>
