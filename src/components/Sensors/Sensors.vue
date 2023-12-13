<template>
  <div id="Sensors">
    <Accelerometer ref="accelerometer" :flagMoving="flagMoving"/>
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
    </p>
  </div>
</template>

<script>
import MovingButton from './MovingButton.vue';

export default Vue.defineComponent({
  name: "Sensors",
  components: {
    Accelerometer: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/Accelerometer.vue", options)),
    PermissionRequester: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/PermissionRequester.vue", options)),
    MovingButton: Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/MovingButton.vue", options)),
    MovingButton
},
  data() {
    return {
      acceleration: [0, 0, 0],
      device2room: Quaternion.identity,
      flagMoving: false,
    };
  },
  methods: {
    ReadAccelerometer() {
        this.acceleration = this.$refs["accelerometer"].GetAccelerationInRoom()
    
        this.device2room = this.$refs["accelerometer"].GetDevice2RoomQuaternion()
    },
    StartMovement() {
        this.flagMoving = true
    },
    StopMovement() {
        this.flagMoving = false
    },
  },
});
</script>
