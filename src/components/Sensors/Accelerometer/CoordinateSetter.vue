<template>
    <div id="CoordinateSetter">
        <button @click="SetCoordinate">
            {{ t('CoordinateSetter.Set') }}
        </button>
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'CoordinateSetter',
    components: {
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
    methods: {
        SetCoordinate() {
            this.$emit('set')
        }
    },
})
</script>

<i18n>
{
    "en": {
        "CoordinateSetter": {
            "Set": "Set base coordinate"
        }
    },
    "ja": {
        "CoordinateSetter": {
            "Set": "座標をセット"
        }
    }
}
</i18n>