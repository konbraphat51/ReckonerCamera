<template>
    <div id="Accelerometer">
        <p>
            x: {{ accelerometerData.x }} <br>
            y: {{ accelerometerData.y }} <br>
            z: {{ accelerometerData.z }} <br>
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
        },
    }
</script>

<i18n>
</i18n>