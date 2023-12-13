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

      <h3>Direction</h3>
        x: {{ direction[0] }} <br />
        y: {{ direction[1] }} <br />
        z: {{ direction[2] }} <br />

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
      direction: [0, 0, 0]
    };
  },
  methods: {
    GetAcceleration() {
        this.acceleration = this.$refs["accelerometer"].GetAccelerationInRoom()
    
        let q = this.$refs["accelerometer"].GetDevice2RoomQuaternion()
        this.direction = q.RotateVector([0, 1, 0])
    },
  },
});
</script>
