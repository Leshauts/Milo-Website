<template>
    <section class="section-audio-sources">
        <div class="section-title">
            <div class="section-title__tag body-small">
                Audio Sources
            </div>
            <h2 class="section-title__title h2">
                Play audio from <span class="no-wrap"><img :src="spotifyIcon" alt="Spotify"
                        class="inline-icon" />Spotify</span>, <span class="no-wrap"><img :src="bluetoothIcon"
                        alt="Bluetooth" class="inline-icon" />Bluetooth</span> or&nbsp;your <span class="no-wrap"><img
                        :src="macosIcon" alt="Mac" class="inline-icon" />Mac</span>.
            </h2>
        </div>

        <div class="audio-sources-display">
            <div class="video-container">
                <video v-for="(video, index) in videos" :key="index" :ref="el => setVideoRef(el, index)" :src="video"
                    class="video-element" :class="{ 'video-active': index === activeButtonIndex }"
                    :autoplay="index === 0" loop muted playsinline @loadeddata="onVideoLoaded(index)"
                    @canplay="onVideoCanPlay(index)">
                </video>
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
import spotifyIcon from '../assets/icons/spotify-mono.svg'
import bluetoothIcon from '../assets/icons/bluetooth-mono.svg'
import macosIcon from '../assets/icons/macos-mono.svg'
import spotifyColor from '../assets/icons/spotify-color.svg'
import bluetoothColor from '../assets/icons/bluetooth-color.svg'
import macosColor from '../assets/icons/macos-color.svg'

export default {
    name: 'SectionAudioSources',
    data() {
        return {
            spotifyIcon,
            bluetoothIcon,
            macosIcon,
            activeButtonIndex: 0,
            videoRefs: {},
            videosLoaded: {},
            videos: [
                '/src/assets/videos/audio-sources/spotify-demo.mp4',
                '/src/assets/videos/audio-sources/bluetooth-demo.mp4',
                '/src/assets/videos/audio-sources/mac-audio-demo.mp4'
            ],
            buttons: [
                { name: 'Spotify Connect', iconColor: spotifyColor, iconMono: spotifyIcon },
                { name: 'Bluetooth', iconColor: bluetoothColor, iconMono: bluetoothIcon },
                { name: 'Mac Audio Receiver', iconColor: macosColor, iconMono: macosIcon }
            ]
        }
    },
    watch: {
        activeButtonIndex(newIndex, oldIndex) {
            this.manageVideoPlayback(newIndex, oldIndex)
        }
    },
    methods: {
        setActiveButton(index) {
            this.activeButtonIndex = index

            // Scroll automatique sur mobile
            this.$nextTick(() => {
                // Vérifier si on est sur mobile (même breakpoint que le CSS)
                const isMobile = window.matchMedia('(max-width: 600px)').matches

                if (isMobile) {
                    const scrollWrapper = this.$el.querySelector('.buttons-scroll-wrapper')
                    const buttons = this.$el.querySelectorAll('.audio-button')
                    const clickedButton = buttons[index]

                    if (scrollWrapper && clickedButton) {
                        // Calculer la position pour centrer le bouton
                        const buttonLeft = clickedButton.offsetLeft
                        const buttonWidth = clickedButton.offsetWidth
                        const wrapperWidth = scrollWrapper.offsetWidth

                        // Position pour centrer le bouton
                        const scrollPosition = buttonLeft - (wrapperWidth / 2) + (buttonWidth / 2)

                        // Scroll manuel du conteneur spécifique
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
                    console.log('Video play failed:', error)
                })
            }
        },

        manageVideoPlayback(activeIndex, previousIndex) {
            // Pause and reset all videos except the active one
            Object.keys(this.videoRefs).forEach(index => {
                const video = this.videoRefs[index]
                const idx = parseInt(index)
                if (video && idx !== activeIndex) {
                    video.pause()
                    video.currentTime = 0
                }
            })

            // Play the active video
            this.playActiveVideo()
        },

        onVideoLoaded(index) {
            console.log(`Video ${index} loaded and ready`)
            this.videosLoaded[index] = true

            // If this is not the active video, pause it and reset to beginning
            if (index !== this.activeButtonIndex) {
                const video = this.videoRefs[index]
                if (video) {
                    video.pause()
                    video.currentTime = 0
                }
            }
        },

        onVideoCanPlay(index) {
            console.log(`Video ${index} can play`)
            // Make sure the active video is playing
            if (index === this.activeButtonIndex) {
                const video = this.videoRefs[index]
                if (video && video.paused) {
                    video.currentTime = 0
                    video.play().catch(error => {
                        console.log('Video autoplay failed:', error)
                    })
                }
            }
        }
    }
}
</script>

<style scoped>
.section-audio-sources {
    position: relative;
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    background-color: var(--color-background-neutral);
    border-radius: var(--border-radius-large);
    padding: var(--space-09);
    overflow: hidden;
}

.section-title {
    grid-column: 3 / -3;
    text-align: center;
    padding-bottom: var(--space-07);
    z-index: 2;
}

.section-title__tag {
    color: var(--color-brand);
    margin-bottom: var(--space-03);
}

.section-title__title {
    color: var(--color-text);
}

.inline-icon {
    width: 32px;
    height: 32px;
    vertical-align: middle;
    margin-right: var(--space-02);
}

.no-wrap {
    white-space: nowrap;
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
}

.video-element {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    opacity: 0;
    transition: opacity 0.8s ease-in-out;
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
}

.buttons-scroll-wrapper {
    flex-shrink: 0;
}

.buttons-container {
    display: flex;
    flex-direction: column;
    gap: var(--space-02);
    align-items: flex-start;
}

.audio-button {
    background: var(--color-background-neutral-32);
    width: 100%;
    border: none;
    box-shadow: 0px 2px 32px rgba(0, 0, 0, 0.08);
    color: var(--color-text-secondary);
    padding: var(--space-03) var(--space-06) var(--space-03) var(--space-03);
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
    top: 64%;
    left: 24%;
    transform: translate(-50%, -50%);
    z-index: 0;
    pointer-events: none;
    width: 64vw;
    height: 64vw;
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
    width: 64vw;
    height: 64vw;
    top: 0;
    left: 0;
    filter: blur(calc(64vw / 5));
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

@media (max-width: 680px) {
    .animated-gradients {
        left: 50%;
        width: 120vw;
        height: 120vw;
    }

    .gradient {
        width: 120vw;
        height: 120vw;
        filter: blur(calc(120vw / 5));
    }
}

@media (max-width: 1024px) {
    .section-audio-sources {
        padding: var(--space-09) var(--space-07);
    }

    .audio-sources-display {
        grid-column: 1 / -1;
        width: 100%;
        gap: var(--space-06);
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

    .inline-icon {
        width: 24px;
        height: 24px;
    }
}

@media (max-width: 680px) {
    .section-audio-sources {
        padding: var(--space-09) var(--space-03);
    }

    .audio-sources-display {
        display: flex;
        flex-direction: column-reverse;
        gap: inherit;
    }

    .section-title {
        grid-column: 1 / -1;
        padding-bottom: var(--space-03);
    }

    /* Modifications pour le scroll horizontal */
    .buttons-scroll-wrapper {
        width: calc(100% + var(--space-06));
        overflow-x: auto;
        overflow-y: hidden;
        -webkit-overflow-scrolling: touch;
        /* Pour iOS */
    }

    .buttons-scroll-wrapper::-webkit-scrollbar {
        display: none;
    }

    .buttons-container {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
        flex-wrap: nowrap;
        gap: var(--space-04);
        width: 100%;
        /* Important : permet au container de s'étendre */
        padding: var(--space-06);
        /* Padding calculé pour centrer le premier bouton */

    }

    .audio-button {
        width: auto;
    }

    .button-icon-container {
        width: 24px;
        height: 24px;
    }

    .inline-icon {
        width: 20px;
        height: 20px;
    }
}
</style>