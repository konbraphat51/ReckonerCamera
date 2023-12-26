<template>
    <div id="Accelerometer">
        <p id="AccelerometerSensor">
            <h3 @click="ToggleShowingAcceleration"> {{ t("accelerometer.accelerometer") }} </h3>
            <div v-if="flagShowingAcceleraion">
                x: {{ accelerationInRoom[0] }} <br>
                y: {{ accelerationInRoom[1] }} <br>
                z: {{ accelerationInRoom[2] }} <br>
                <label> {{ t("accelerometer.negate") }} </label>
                <input type="checkbox" v-model="flagNegatingAcceleration" />
            </div>
        </p>

        <p id="Direction">
            <h3 @click="ToggleShowingDirection"> {{ t("accelerometer.direction") }} </h3>
            <div v-if="flagShowingDirection">
                x: {{ direction[0] }} <br>
                y: {{ direction[1] }} <br>
                z: {{ direction[2] }} <br>
            </div>

            <DirectionSetter @set="SetDeviceDirection" />
        </p>

        <DistanceCalculater 
            @distanceCalculated="OnDistanceCalculated" 
            :flagMoving="flagMoving"
            :accelerationInRoom="accelerationInRoom"/>

        <CoordinateSetter @set="SetCoordinate" />
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Accelerometer',
    components: {
    "CoordinateSetter": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/CoordinateSetter.vue", options)),
    "DistanceCalculater": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/DistanceCalculater.vue", options)),
    "DirectionSetter": Vue.defineAsyncComponent(() => loadModule("src/components/Sensors/Accelerometer/DirectionSetter.vue", options)),
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
            deviceDirection: [0, 1, 0],
            flagListening: false,
            earth2roomQuaternion: Quaternion.identity,
            flagShowingAcceleraion: false,
            flagShowingDirection: true,
            flagNegatingAcceleration: false
        }
    },
    mounted() {
        this.StartListening()
    },
    methods: {
        StartListening() {
            if (window.DeviceMotionEvent) {
                window.addEventListener('devicemotion', this.ReceiveAcceleration, false)
            } else {
                alert("DeviceMotionEvent is not supported")
            }

            if (window.DeviceOrientationEvent) {
                window.addEventListener('deviceorientation', this.ReceiveOrientation, false)
            } else {
                alert("DeviceOrientationEvent is not supported")
            }

            this.flagListening = true
        },
        ReceiveAcceleration(event) {
            if (this.flagNegatingAcceleration) {
                this.accelerometerData.x = -event.acceleration.x
                this.accelerometerData.y = -event.acceleration.y
                this.accelerometerData.z = -event.acceleration.z
            } else {
                this.accelerometerData.x = event.acceleration.x
                this.accelerometerData.y = event.acceleration.y
                this.accelerometerData.z = event.acceleration.z
            }
        },
        ReceiveOrientation(event) {
            this.orientationData.alpha = event.alpha
            this.orientationData.beta = event.beta
            this.orientationData.gamma = event.gamma
        },
        SetCoordinate() {
            //clone
            this.earth2roomQuaternion = Quaternion.Inverse(this.device2EarthQuaternion).normalized

            this.$emit("coordinateSet")

            alert("Coordinate set")
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
        },
        ToggleShowingAcceleration() {
            this.flagShowingAcceleraion = !this.flagShowingAcceleraion
        },
        ToggleShowingDirection() {
            this.flagShowingDirection = !this.flagShowingDirection
        },
        SetDeviceDirection(deviceDirection) {
            this.deviceDirection = deviceDirection
        }
    },
    computed: {
        device2EarthQuaternion() {
            //Device orientation:
            // https://developer.mozilla.org/ja/docs/Web/API/Device_orientation_events/Orientation_and_motion_data_explained
            let earth2deviceAlpha = Quaternion.AngleAxis(-this.orientationData.alpha, [0, 0, 1])
            let earth2deviceBeta = Quaternion.AngleAxis(-this.orientationData.beta, [1, 0, 0])
            let earth2deviceGamma = Quaternion.AngleAxis(-this.orientationData.gamma, [0, 1, 0])

            let earth2deviceQuaternion = Quaternion.Multiply(earth2deviceAlpha, earth2deviceBeta, earth2deviceGamma).inversed

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
            return this.device2roomQuaternion.RotateVector([this.deviceDirection[0], this.deviceDirection[1], this.deviceDirection[2]])
        }
    }
})
</script>

<i18n>
{
    "en": {
        "accelerometer": {
            "accelerometer": "Accelerometer (InRoom Coordinate)",
            "direction": "Device Direction (InRoom Coordinate)",
            "negate": "Negate"
        }
    },
    "ja": {
        "accelerometer": {
            "accelerometer": "加速度センサー (部屋内座標)",
            "direction": "デバイスの向き (部屋内座標)",
            "negate": "反転"
        }
    }
}
</i18n>

<style>
#AccelerometerSensor {
    background-color:lightblue;
    border-radius: 10px;
    padding: 5px;
}

#Direction {
    background-color:lightgreen;
    border-radius: 10px;
    padding: 5px;
}

</style>