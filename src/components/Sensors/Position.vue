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
		Move(movementVector) {
			this.x += movementVector[0]
			this.y += movementVector[1]
			this.z += movementVector[2]
		},
		GetPosition() {
			return [this.x, this.y, this.z]
		},
		Reset() {
			this.x = 0
			this.y = 0
			this.z = 0
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

<style>
#position {
	background-color: pink;
	border-radius: 10px;
	padding: 5px;
}
</style>
