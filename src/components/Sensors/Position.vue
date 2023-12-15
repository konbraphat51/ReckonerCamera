<template>
	<div id="position">
		<h3>{{ t("position.title") }}</h3>
		<p>
			x: {{ x }} <br />
			y: {{ y }} <br />
			z: {{ z }} <br />
		</p>
	</div>
</template>

<script>
export default Vue.defineComponent({
	name: "Position",
	components: {},
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
	data() {
		return {
			x: 0,
			y: 0,
			z: 0,
		}
	},
	methods: {
		Move(movementVector, altitude) {
			this.x += movementVector[0]
			this.y += movementVector[1]
			this.z = altitude
		},
		GetPosition() {
			return [this.x, this.y, this.z]
		},
	},
})
</script>

<i18n>
    {
        "en": {
            "position": {
                "title": "Position"
            }
        },
        "ja": {
            "position": {
                "title": "座標"
            }
        }
    }
</i18n>
