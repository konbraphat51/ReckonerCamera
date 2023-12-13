<template>
  <div id="Sensors">
    <Accelerometer ref="accelerometer" />
    <PermissionRequester />

    <button @click="GetAcceleration">Get Acceleration</button>

    <p>
      <h3>Acceleration</h3>
      x: {{ acceleration[0] }} <br />
      y: {{ acceleration[1] }} <br />
      z: {{ acceleration[2] }} <br />
    </p>
  </div>
</template>

<script>
export default Vue.defineComponent({
  name: "Sensors",
  components: {
    Accelerometer: Vue.defineAsyncComponent(() =>
      loadModule(
        "src/components/Sensors/Accelerometer/Accelerometer.vue",
        options
      )
    ),
    PermissionRequester: Vue.defineAsyncComponent(() =>
      loadModule("src/components/Sensors/PermissionRequester.vue", options)
    ),
  },
  data() {
    return {
      acceleration: [0, 0, 0],
    };
  },
  methods: {
    GetAcceleration() {
      this.acceleration = this.$refs["accelerometer"].GetAccelerationInRoom()
    },
  },
});
</script>
