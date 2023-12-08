<template>
    <div id="app">
        <LanguageSelection />
        <Camera />
        <Sensors />
    </div>
</template>

<script>
import Sensors from './Sensor/Sensors.vue';

export default Vue.defineComponent({
    name: 'App',
    components: {
        "LanguageSelection": Vue.defineAsyncComponent(() => loadModule("src/components/LanguageSelection.vue", options)),
        "Camera": Vue.defineAsyncComponent(() => loadModule("src/components/Camera/Camera.vue", options)),
        "Sensors": Vue.defineAsyncComponent(() => loadModule("src/components/Sensor/Sensors.vue", options)),
        Sensors
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
})
</script>