<template>
    <section class="section-video">
        <div class="section-video__container">
            <video 
                ref="videoElement"
                class="section-video__video"
                :src="currentVideoSrc"
                @play="isPlaying = true"
                @pause="isPlaying = false"
                muted
                loop
                playsinline
            >
                Votre navigateur ne supporte pas les vidéos HTML5.
            </video>
            
            <!-- Contrôles vidéo -->
            <div class="section-video__controls">
                <div class="section-video__control-left">
                    <ButtonIcon 
                        :icon="isPlaying ? 'pause' : 'play'"
                        @click="togglePlayPause"
                    />
                </div>
                
                <div class="section-video__control-right">
                    <ButtonIcon 
                        icon="fullscreen"
                        @click="toggleFullscreen"
                    />
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import ButtonIcon from './ButtonIcon.vue'

export default {
    name: 'SectionVideo',
    components: {
        ButtonIcon
    },
    props: {
        desktopVideoSrc: {
            type: String,
            required: true
        },
        mobileVideoSrc: {
            type: String,
            required: true
        },
        autoplay: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            isPlaying: false,
            isMobile: false
        }
    },
    computed: {
        currentVideoSrc() {
            return this.isMobile ? this.mobileVideoSrc : this.desktopVideoSrc
        }
    },
    mounted() {
        this.checkScreenSize()
        window.addEventListener('resize', this.checkScreenSize)
        
        if (this.autoplay) {
            this.$nextTick(() => {
                this.playVideo()
            })
        }
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.checkScreenSize)
    },
    methods: {
        checkScreenSize() {
            const wasMobile = this.isMobile
            this.isMobile = window.innerWidth <= 1024
            
            // Si on change de device, on recharge la vidéo avec la bonne source
            if (wasMobile !== this.isMobile) {
                this.$nextTick(() => {
                    const wasPlaying = this.isPlaying
                    this.$refs.videoElement.load()
                    if (wasPlaying) {
                        this.$refs.videoElement.addEventListener('loadeddata', () => {
                            this.playVideo()
                        }, { once: true })
                    }
                })
            }
        },
        togglePlayPause() {
            if (this.isPlaying) {
                this.pauseVideo()
            } else {
                this.playVideo()
            }
        },
        playVideo() {
            this.$refs.videoElement.play().catch(error => {
                console.log('Erreur lors de la lecture:', error)
            })
        },
        pauseVideo() {
            this.$refs.videoElement.pause()
        },
        toggleFullscreen() {
            const video = this.$refs.videoElement
            
            if (!document.fullscreenElement) {
                video.requestFullscreen().catch(error => {
                    console.log('Erreur fullscreen:', error)
                })
            } else {
                document.exitFullscreen()
            }
        }
    }
}
</script>

<style scoped>
.section-video {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    margin-bottom: var(--space-08);
}

.section-video__container {
    grid-column: 1 / -1;
    position: relative;
    border-radius: var(--border-radius-large);
    overflow: hidden;
    background-color: var(--color-background-contrast);
    
    /* Ratio 2:1 pour desktop */
    aspect-ratio: 2 / 1;
}

.section-video__video {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}

.section-video__controls {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    padding: var(--space-03);
    pointer-events: auto;
    z-index: 10;
}

.section-video__control-left,
.section-video__control-right {
    display: flex;
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
    .section-video__container {
        /* Ratio 5:7 pour mobile */
        aspect-ratio: 5 / 7;
    }
    
}

/* Styles pour le mode fullscreen */
.section-video__video:fullscreen {
    object-fit: contain;
}
</style>