<template>
    <div id="Button">
        <button @click="takePicture">{{ t('camera.takePicture') }}</button>
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Camera',
    components: {
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
    methods: {
        takePicture() {
            this.$emit('takePicture')
        }
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