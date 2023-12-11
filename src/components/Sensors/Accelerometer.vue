<template>
    <div id="Accelerometer">
        <p>
            x: {{ accelerometerData.x }} <br>
            y: {{ accelerometerData.y }} <br>
            z: {{ accelerometerData.z }} <br>

            xMax: {{ accelerometerData.xMax }} <br>
            yMax: {{ accelerometerData.yMax }} <br>
            zMax: {{ accelerometerData.zMax }} <br>
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
                z: 0,
                xMax: 0,
                yMax: 0,
                zMax: 0,
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
        },
        GetAcceleration(event) {
            this.accelerometerData.x = event.acceleration.x
            this.accelerometerData.y = event.acceleration.y
            this.accelerometerData.z = event.acceleration.z

            if (Math.abs(this.accelerometerData.x) > Math.abs(this.accelerometerData.xMax)) {
                this.accelerometerData.xMax = this.accelerometerData.x
            }
            if (Math.abs(this.accelerometerData.y) > Math.abs(this.accelerometerData.yMax)) {
                this.accelerometerData.yMax = this.accelerometerData.y
            }
            if (Math.abs(this.accelerometerData.z) > Math.abs(this.accelerometerData.zMax)) {
                this.accelerometerData.zMax = this.accelerometerData.z
            }
        },
    }
})
</script>