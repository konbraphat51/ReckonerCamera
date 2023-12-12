<template>
    <div id="Accelerometer">
        <p>
        <h3>Accelerometer</h3>
        x: {{ accelerometerData.x }} <br>
        y: {{ accelerometerData.y }} <br>
        z: {{ accelerometerData.z }} <br>

        <h3>Orientation</h3>
        alpha: {{ orientationData.alpha }} <br>
        beta: {{ orientationData.beta }} <br>
        gamma: {{ orientationData.gamma }} <br>
        </p>
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Accelerometer',
    components: {
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
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
            flagListening: false
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
        }
    }
})
</script>