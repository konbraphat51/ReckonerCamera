<template>
    <div id="Accelerometer">
        <p>
        <h3>Accelerometer</h3>
        x: {{ accelerationInRoom[0] }} <br>
        y: {{ accelerationInRoom[1] }} <br>
        z: {{ accelerationInRoom[2] }} <br>
        </p>

        <p>
        <h3>Direction</h3>
        x: {{ direction[0] }} <br>
        y: {{ direction[1] }} <br>
        z: {{ direction[2] }} <br>
        </p>

        <CoordinateSetter @set="SetCoordinate" />
        <DistanceCalculater @distanceCalculated="OnDistanceCalculated" @flagMoving="flagMoving"/>
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Accelerometer',
    components: {
    "CoordinateSetter": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/CoordinateSetter.vue", options)),
    "DistanceCalculater": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/DistanceCalculater.vue", options)),
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
    props: {
        flagMoving: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            accelerometerData: {
                x: 0,
                y: 0,
                z: 0
            },
            orientationData: {
                alpha: 0,
                beta: 0,
                gamma: 0
            },
            flagListening: false,
            earth2roomQuaternion: Quaternion.identity,
        }
    },
    mounted() {
        this.StartListening()
    },
    methods: {
        StartListening() {
            if (window.DeviceMotionEvent) {
                window.addEventListener('devicemotion', this.GetAcceleration, false)
            } else {
                alert("DeviceMotionEvent is not supported")
            }

            if (window.DeviceOrientationEvent) {
                window.addEventListener('deviceorientation', this.GetOrientation, false)
            } else {
                alert("DeviceOrientationEvent is not supported")
            }
        },
        GetAcceleration(event) {
            this.accelerometerData.x = event.acceleration.x
            this.accelerometerData.y = event.acceleration.y
            this.accelerometerData.z = event.acceleration.z
        },
        GetOrientation(event) {
            this.orientationData.alpha = event.alpha
            this.orientationData.beta = event.beta
            this.orientationData.gamma = event.gamma
        },
        SetCoordinate() {
            this.earth2roomQuaternion = this.earth2DeviceQuaternion
        },
        OnDistanceCalculated(distance) {
            this.$emit("distanceCalculated", distance)
        },
        GetAccelerationInRoom() {
            return this.accelerationInRoom
        },
        GetDevice2RoomQuaternion() {
            return this.device2roomQuaternion
        },
        GetDevice2EarthQuaternion() {
            return this.device2EarthQuaternion
        }
    },
    computed: {
        device2EarthQuaternion() {
            //Device orientation:
            // https://developer.mozilla.org/ja/docs/Web/API/Device_orientation_events/Orientation_and_motion_data_explained
            let earth2deviceAlpha = Quaternion.AngleAxis(-this.orientationData.alpha, [0, 0, 1])
            let earth2deviceBeta = Quaternion.AngleAxis(-this.orientationData.beta, [1, 0, 0])
            let earth2deviceGamma = Quaternion.AngleAxis(-this.orientationData.gamma, [0, 1, 0])

            let earth2deviceQuaternion = Quaternion.Multiply(earth2deviceGamma, Quaternion.Multiply(earth2deviceBeta, earth2deviceAlpha))

            return Quaternion.Inverse(earth2deviceQuaternion).normalized
        },
        device2roomQuaternion() {
            return Quaternion.Multiply(this.earth2roomQuaternion, this.device2EarthQuaternion).normalized
        },
        accelerationInRoom() {
            let accelerationInDevice = [this.accelerometerData.x, this.accelerometerData.y, this.accelerometerData.z]
            
            return this.device2roomQuaternion.RotateVector(accelerationInDevice)
        },
        direction() {
            return this.device2roomQuaternion.RotateVector([0, 1, 0])
        }
    }
})
</script>