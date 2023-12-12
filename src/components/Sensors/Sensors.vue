<template>
    <div id="Sensors">
        <Accelerometer />
        <PermissionRequester />
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Sensors',
    components: {
        "Accelerometer": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/Accelerometer.vue", options)),
        "PermissionRequester": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/PermissionRequester.vue", options)),
    }
})
</script>