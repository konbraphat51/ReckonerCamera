<template>
  <div id="Sensors">
    <Accelerometer ref="accelerometer" :flagMoving="flagMoving" @distanceCalculated="OnDistanceCalculated"/>
    <PermissionRequester />
    <MovingButton
      @startMovement="StartMovement"
      @stopMovement="StopMovement"
      />

    <button @click="ReadAccelerometer">Get Acceleration</button>

    <p>
      <h3>Acceleration</h3>
      x: {{ acceleration[0] }} <br />
      y: {{ acceleration[1] }} <br />
      z: {{ acceleration[2] }} <br />

      <h3>altitude</h3>
      {{ altitude }}
    </p>

    <Position ref="position" />
  </div>
</template>

<script>
export default Vue.defineComponent({
  name: "Sensors",
  components: {
    Accelerometer: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/Accelerometer.vue", options)),
    PermissionRequester: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/PermissionRequester.vue", options)),
    MovingButton: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/MovingButton.vue", options)),
    Position: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Position.vue", options)),
},
  data() {
    return {
      acceleration: [0, 0, 0],
      device2room: Quaternion.identity,
      altitude: 0,
      flagMoving: false,
    };
  },
  methods: {
    ReadAccelerometer() {
        this.acceleration = this.$refs["accelerometer"].GetAccelerationInRoom()
    
        this.device2room = this.$refs["accelerometer"].GetDevice2RoomQuaternion()

        this.altitude = this.$refs["accelerometer"].GetAltitude()
    },
    StartMovement() {
        this.flagMoving = true
    },
    StopMovement() {
        this.flagMoving = false
    },
    OnDistanceCalculated(distance, altitude) {
      this.$refs["position"].Move(distance, altitude)
    },
    GetPosition() {
      return this.$refs["position"].GetPosition()
    },
    GetRoom2Device() {
      let device2room = this.$refs["accelerometer"].GetDevice2RoomQuaternion()
      return Quaternion.Inverse(device2room).normalized
    }
  },
});
</script>
