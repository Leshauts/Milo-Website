<template>
    <section class="section-audio-sources">
        <SectionTitle :tag="tag" align="center" size="section">
            <template #title>
                {{ titlePrefix }} <span class="no-wrap"><img :src="spotifyIcon" alt="Spotify"
                        class="inline-icon" />Spotify</span>, <span class="no-wrap"><img :src="bluetoothIcon"
                        alt="Bluetooth" class="inline-icon" />Bluetooth</span> or&nbsp;your <span class="no-wrap"><img
                        :src="macosIcon" alt="Mac" class="inline-icon" />Mac</span>.
            </template>
        </SectionTitle>

        <div class="audio-sources-display">
            <div class="video-container">
                <!-- Mode Desktop : Multiple vidéos avec crossfade -->
                <template v-if="!isMobile">
                    <video v-for="(video, index) in videos" :key="index" :ref="el => setVideoRef(el, index)"
                        :src="video" class="video-element" :class="{
                            'video-active': index === activeButtonIndex,
                            'video-previous': index === previousButtonIndex && previousButtonIndex !== null
                        }" :autoplay="index === 0" loop muted playsinline @loadeddata="onVideoLoaded(index)"
                        @canplay="onVideoCanPlay(index)">
                    </video>
                </template>

                <!-- Mode Mobile : Une seule vidéo avec fade simple -->
                <template v-else>
                    <video ref="mobileVideo" :src="mobileVideoSrc" class="video-element mobile-video"
                        :class="{ 'mobile-video-hidden': isTransitioning }" autoplay loop muted playsinline
                        webkit-playsinline preload="auto" @canplay="onMobileVideoReady">
                    </video>
                </template>
            </div>

            <div class="buttons-scroll-wrapper">
                <div class="buttons-container">
                    <button v-for="(button, index) in buttons" :key="index" class="audio-button"
                        :class="{ 'audio-button--active': activeButtonIndex === index }"
                        @click="setActiveButton(index)">
                        <div class="button-icon-container">
                            <img :src="button.iconMono" :alt="button.name + ' mono icon'"
                                class="button-icon button-icon--mono" />
                            <img :src="button.iconColor" :alt="button.name + ' color icon'"
                                class="button-icon button-icon--color" />
                        </div>
                        {{ button.name }}
                    </button>
                </div>
            </div>
        </div>

        <div class="animated-gradients">
            <div class="gradient gradient-0" :class="{ 'gradient-active': activeButtonIndex === 0 }"></div>
            <div class="gradient gradient-1" :class="{ 'gradient-active': activeButtonIndex === 1 }"></div>
            <div class="gradient gradient-2" :class="{ 'gradient-active': activeButtonIndex === 2 }"></div>
        </div>
    </section>
</template>

<script>
import SectionTitle from './SectionTitle.vue'
import spotifyIcon from '../assets/icons/spotify-mono.svg'
import bluetoothIcon from '../assets/icons/bluetooth-mono.svg'
import macosIcon from '../assets/icons/macos-mono.svg'
import spotifyColor from '../assets/icons/spotify-color.svg'
import bluetoothColor from '../assets/icons/bluetooth-color.svg'
import macosColor from '../assets/icons/macos-color.svg'

export default {
    name: 'SectionAudioSources',
    components: {
        SectionTitle
    },
    props: {
        tag: {
            type: String,
            required: true
        },
        titlePrefix: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            spotifyIcon,
            bluetoothIcon,
            macosIcon,
            activeButtonIndex: 0,
            previousButtonIndex: null,
            isMobile: false,
            isTransitioning: false,
            mobileVideoSrc: '/src/assets/videos/spotify-demo.mp4',
            videoRefs: {},
            videosLoaded: {},
            videos: [
                '/src/assets/videos/spotify-demo.mp4',
                '/src/assets/videos/bluetooth-demo.mp4',
                '/src/assets/videos/mac-audio-demo.mp4'
            ],
            buttons: [
                { name: 'Spotify', iconColor: spotifyColor, iconMono: spotifyIcon },
                { name: 'Bluetooth', iconColor: bluetoothColor, iconMono: bluetoothIcon },
                { name: 'Mac Audio', iconColor: macosColor, iconMono: macosIcon }
            ]
        }
    },
    mounted() {
        this.isMobile = this.detectMobile()
        console.log('Mobile device detected:', this.isMobile)
    },
    watch: {
        activeButtonIndex(newIndex, oldIndex) {
            if (!this.isMobile) {
                this.manageDesktopVideoPlayback(newIndex, oldIndex)
            } else {
                this.manageMobileVideoPlayback()
            }
        }
    },
    methods: {
        detectMobile() {
            const userAgent = navigator.userAgent || navigator.vendor || window.opera
            const isIOS = /iPad|iPhone|iPod/.test(userAgent) && !window.MSStream
            const isAndroid = /android/i.test(userAgent)
            const isMobileUserAgent = /Mobi|Android/i.test(userAgent)

            return isIOS || isAndroid || isMobileUserAgent
        },

        setActiveButton(index) {
            this.activeButtonIndex = index

            this.$nextTick(() => {
                const isMobileScreen = window.matchMedia('(max-width: 600px)').matches

                if (isMobileScreen) {
                    const scrollWrapper = this.$el.querySelector('.buttons-scroll-wrapper')
                    const buttons = this.$el.querySelectorAll('.audio-button')
                    const clickedButton = buttons[index]

                    if (scrollWrapper && clickedButton) {
                        const buttonLeft = clickedButton.offsetLeft
                        const buttonWidth = clickedButton.offsetWidth
                        const wrapperWidth = scrollWrapper.offsetWidth
                        const scrollPosition = buttonLeft - (wrapperWidth / 2) + (buttonWidth / 2)

                        scrollWrapper.scrollTo({
                            left: scrollPosition,
                            behavior: 'smooth'
                        })
                    }
                }
            })
        },

        setVideoRef(el, index) {
            if (el) {
                this.videoRefs[index] = el
            }
        },

        playActiveVideo() {
            const activeVideo = this.videoRefs[this.activeButtonIndex]
            if (activeVideo && this.videosLoaded[this.activeButtonIndex]) {
                activeVideo.currentTime = 0
                activeVideo.play().catch(error => {
                    console.log('Desktop video play failed:', error)
                })
            }
        },

        manageDesktopVideoPlayback(activeIndex, previousIndex) {
            console.log(`Desktop crossfade: ${previousIndex} → ${activeIndex}`)

            this.previousButtonIndex = previousIndex

            Object.keys(this.videoRefs).forEach(index => {
                const video = this.videoRefs[index]
                const idx = parseInt(index)
                if (video && idx !== activeIndex && idx !== previousIndex) {
                    video.pause()
                    video.currentTime = 0
                }
            })

            this.playActiveVideo()

            setTimeout(() => {
                if (this.previousButtonIndex !== null) {
                    const previousVideo = this.videoRefs[this.previousButtonIndex]
                    if (previousVideo) {
                        previousVideo.pause()
                        previousVideo.currentTime = 0
                    }
                    this.previousButtonIndex = null
                }
            }, 400)
        },

        onVideoLoaded(index) {
            console.log(`Desktop video ${index} loaded`)
            this.videosLoaded[index] = true

            if (index !== this.activeButtonIndex) {
                const video = this.videoRefs[index]
                if (video) {
                    video.pause()
                    video.currentTime = 0
                }
            }
        },

        onVideoCanPlay(index) {
            console.log(`Desktop video ${index} can play`)
            if (index === this.activeButtonIndex) {
                const video = this.videoRefs[index]
                if (video && video.paused) {
                    video.currentTime = 0
                    video.play().catch(error => {
                        console.log('Desktop video autoplay failed:', error)
                    })
                }
            }
        },

        manageMobileVideoPlayback() {
            if (this.isTransitioning) return

            console.log(`Mobile fade-out current video`)

            this.isTransitioning = true

            setTimeout(() => {
                console.log(`Mobile changing src to video ${this.activeButtonIndex}`)
                this.mobileVideoSrc = this.videos[this.activeButtonIndex]
            }, 200)
        },

        onMobileVideoReady() {
            if (!this.isTransitioning) return

            console.log(`Mobile video ready, fade-in`)

            const video = this.$refs.mobileVideo
            if (video) {
                video.currentTime = 0
                video.play().then(() => {
                    this.isTransitioning = false
                }).catch(() => {
                    this.isTransitioning = false
                })
            } else {
                this.isTransitioning = false
            }
        }
    }
}
</script>

<style scoped>
.no-wrap {
    display: inline-block;
}

.inline-icon {
    width: 32px;
    margin-right: var(--space-01);
    margin-left: 2px;
}


.section-audio-sources {
    position: relative;
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    row-gap: var(--space-08);
    background-color: var(--color-background-neutral);
    border-radius: var(--border-radius-large);
    padding: var(--space-09);
    overflow: hidden;
}


.audio-sources-display {
    display: flex;
    grid-column: 2 / -2;
    gap: var(--space-07);
    align-items: center;
    justify-content: center;
    z-index: 2;
}

.video-container {
    position: relative;
    border-radius: var(--border-radius-xxlarge);
    overflow: hidden;
    max-width: 1024px;
    aspect-ratio: 7 / 4;
    box-shadow: 0px 12px 64px rgba(0, 0, 0, 0.16);
    flex: 1;
    min-height: 200px;
    width: 100%;
}

.video-element {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    opacity: 0;
    transition: opacity 0.4s ease-in-out;
}

.video-element:first-child {
    position: relative;
}

.video-element:not(:first-child) {
    position: absolute;
    top: 0;
    left: 0;
}

.video-element.video-active {
    opacity: 1;
    z-index: 2;
}

.video-element.video-previous {
    opacity: 0;
    z-index: 1;
}

.mobile-video {
    opacity: 1;
    transition: opacity 0.4s ease-in-out;
    width: 100% !important;
    height: 100% !important;
    object-fit: cover;
}

.mobile-video-hidden {
    opacity: 0;
}

.buttons-scroll-wrapper {
    flex-shrink: 0;
}

.buttons-container {
    display: flex;
    flex-direction: column;
    gap: var(--space-04);
    align-items: flex-start;
}

.audio-button {
    background: var(--color-background-neutral-32);
    width: 100%;
    border: none;
    box-shadow: 0px 2px 32px rgba(0, 0, 0, 0.08);
    color: var(--color-text-secondary);
    padding: var(--space-03) var(--space-07) var(--space-03) var(--space-03);
    border-radius: var(--border-radius-medium);
    cursor: pointer;
    transition: color 0.3s ease;
    font-family: 'Perfectly Nineties', serif;
    font-size: var(--font-size-h5);
    line-height: var(--line-height-h5);
    font-weight: 400;
    display: flex;
    align-items: center;
    gap: var(--space-03);
}

.audio-button--active {
    color: var(--color-text);
    background: var(--color-background-neutral);
}

.button-icon-container {
    position: relative;
    width: 40px;
    height: 40px;
    flex-shrink: 0;
}

.button-icon {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transition: opacity 0.4s ease;
}

.button-icon--mono {
    opacity: 0.3;
}

.button-icon--color {
    opacity: 0;
}

.audio-button--active .button-icon--mono {
    opacity: 0;
}

.audio-button--active .button-icon--color {
    opacity: 1;
}

.animated-gradients {
    position: absolute;
    top: 50%;
    left: 16%;
    transform: translate(-50%, -50%);
    z-index: 0;
    pointer-events: none;
    width: 750px;
    height: 750px;
}

@keyframes rotate {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

.gradient {
    position: absolute;
    top: 0;
    left: 0;
    width: 750px;
    height: 750px;
    filter: blur(150px);
    animation: rotate 15s linear infinite;
    border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
    opacity: 0;
    transition: opacity 0.6s ease;
}

.gradient.gradient-active {
    opacity: 1;
}

.gradient-0 {
    background-image: linear-gradient(rgba(255, 187, 0, 0.4), rgba(231, 162, 0, 0.24));
}

.gradient-1 {
    background-image: linear-gradient(rgba(0, 76, 255, 0.24), rgba(34, 60, 104, 0.08));
}

.gradient-2 {
    background-image: linear-gradient(rgba(163, 173, 194, 0.24), rgba(205, 221, 228, 0.48));
}

@media (max-width: 1024px) {
    .inline-icon {
        width: 24px;
    }

    .section-audio-sources {
        padding: var(--space-09) var(--space-07);
    }

    .audio-sources-display {
        grid-column: 1 / -1;
        width: 100%;
    }

    .buttons-scroll-wrapper::-webkit-scrollbar {
        display: none;
    }

    .audio-button {
        white-space: nowrap;
        flex-shrink: 0;
    }

    .button-icon-container {
        width: 32px;
        height: 32px;
    }
}

@media (max-width: 840px) {
    .section-audio-sources {
        padding: var(--space-09) var(--space-06);
        row-gap: 0;
    }

    .audio-sources-display {
        display: flex;
        flex-direction: column-reverse;
        gap: inherit;
    }

    .buttons-scroll-wrapper {
        width: calc(100% + var(--space-08));
        overflow-x: auto;
        overflow-y: hidden;
        -webkit-overflow-scrolling: touch;
    }

    .buttons-scroll-wrapper::-webkit-scrollbar {
        display: none;
    }

    .buttons-container {
        display: flex;
        flex-direction: row;
        justify-content: center;
        flex-wrap: nowrap;
        gap: var(--space-04);
        width: 100%;
        padding: var(--space-07) var(--space-06) var(--space-05) var(--space-06);

    }

    .buttons-container::after {
        content: '';
        min-width: var(--space-03);
        height: 1px;
    }

    .audio-button {
        width: auto;
    }

    .button-icon-container {
        width: 24px;
        height: 24px;
    }
}

@media (max-width: 600px) {
    .inline-icon {
        width: 20px;
    }

    .section-audio-sources {
        padding: var(--space-09) var(--space-03);
    }

    .buttons-scroll-wrapper {
        width: calc(100% + var(--space-06));
        overflow-x: auto;
        overflow-y: hidden;
        -webkit-overflow-scrolling: touch;
    }

    .buttons-container {
        justify-content: flex-start;
    }
}
</style>