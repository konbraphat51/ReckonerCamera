<template>
    <div id="Camera">
        <video ref="video" autoplay width="300" height="200"></video>
    </div>
</template>

<script>
export default Vue.defineComponent({
    name: 'Camera',
    components: {
    },
    setup() {
        //set up i18n
        const { t } = VueI18n.useI18n()
        return { t }
    },
    mounted() {
        const video = this.$refs.video
        if (navigator.mediaDevices.getUserMedia) {
            navigator.mediaDevices.getUserMedia({ video: true })
                .then(function (stream) {
                    video.srcObject = stream;
                })
                .catch(function (error) {
                    console.log("Something went wrong!");
                });
        }
    },
})
</script>