<template>
    <div id="Sensors">
        <Accelerometer />
        <PermissionRequester />
    </div>
</template>

<script>
import Accelerometer from './Accelerometer.vue';

export default Vue.defineComponent({
    name: 'Sensors',
    components: {
        "Accelerometer": Vue.defineAsyncComponent(() => loadModule("src/components/Sensor/Accelerometer.vue", options)),
        "PermissionRequester": Vue.defineAsyncComponent(() => loadModule("src/components/Sensor/PermissionRequester.vue", options)),
        Accelerometer
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
})
</script>

<i18n>
</i18n>