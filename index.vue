<template>
    <div v-show="show" class="screenshot">
        <a :href="screenshotSrc" ref="screenshotImg" :download="downloadName" mce_href="#"></a>
        <div class="content">
            <img :src="screenshotSrc" alt="">
        </div>
        <div class="mask"></div>
    </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
    props: {
        downloadName: {
            type: String,
            default: 'screenshoot'
        },
        element: {
            type: Boolean,
            default: false
        }
    },
    data () {
        return {
            screenshotSrc: '',
            show: false
        }
    },
    methods: {
        handleShoot () {
            this.show = true
            setTimeout(() => {
                this.show = false
            }, 2000)
            const zone = this.element ? document.querySelector('#list') : document.body
            html2canvas(zone).then((canvas) => {
                const screenshotSrc = canvas.toDataURL("image/png")
                this.screenshotSrc = screenshotSrc
                this.$nextTick().then(() => {
                    this.$refs.screenshotImg.click()
                })
            })
        }
    }
}
</script>

<style scoped>
.mask {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: #000;
    opacity: .6;
    z-index: 1000;
}
.content {
    position: fixed;
    left: 0;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    z-index: 1001;
}
.content img {
    position: fixed;
    animation: screenshoot 2s;
    animation-fill-mode: forwards;
}

@keyframes screenshoot {
    0% {
        width: 100%;
        height: 100%;
        left: 0;
        bottom: 0
    }
    30% {
        width: 50%;
        height: 50%;
        left: 0;
        bottom: 0
    }
    40% {
        width: 50%;
        height: 50%;
        left: 0;
        bottom: 5%
    }
    60% {
        width: 50%;
        height: 50%;
        left: 0;
        bottom: 0
    }
    80% {
        width: 50%;
        height: 50%;
        left: 0;
        bottom: 5%
    }
    100% {
        width: 10%;
        height: 10%;
        left: 0;
        bottom: 0
    }
}

</style>
