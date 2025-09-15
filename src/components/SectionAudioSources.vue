<!-- src / components / SectionAudioSources.vue -->
<template>
    <div class="sticky-container" ref="stickyContainer">
        <section class="section-audio-sources" ref="stickySection">
            <div class="section-title">
                <div class="section-title__tag body-small">
                    Audio Sources
                </div>
                <h2 class="section-title__title h2">
                    Play audio from <span class="no-wrap"><img :src="spotifyIcon" alt="Spotify"
                            class="inline-icon" />Spotify</span>, <span class="no-wrap"><img :src="bluetoothIcon"
                            alt="Bluetooth" class="inline-icon" />Bluetooth</span> or&nbsp;your<span
                        class="no-wrap"><img :src="macosIcon" alt="Mac" class="inline-icon" />Mac</span>.
                </h2>
            </div>

            <div class="audio-sources-display">
                <div class="video-container">
                    <video ref="mainVideo" :src="currentVideoSrc" class="video-element" autoplay loop muted playsinline
                        @loadstart="onVideoChange"></video>
                </div>

                <div class="buttons-container">
                    <button v-for="(button, index) in buttons" :key="index" class="audio-button"
                        :style="{ opacity: activeButtonIndex === index ? 1 : 0.3 }" @click="setActiveButton(index)">
                        {{ button.name }}
                    </button>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import spotifyIcon from '../assets/icons/spotify-mono.svg'
import bluetoothIcon from '../assets/icons/bluetooth-mono.svg'
import macosIcon from '../assets/icons/macos-mono.svg'

export default {
    name: 'SectionAudioSources',
    data() {
        return {
            spotifyIcon,
            bluetoothIcon,
            macosIcon,
            activeButtonIndex: 0,
            videos: [
                '/src/assets/videos/spotify-demo.mp4',
                '/src/assets/videos/bluetooth-demo.mp4',
                '/src/assets/videos/mac-audio-demo.mp4'
            ],
            buttons: [
                { name: 'Spotify' },
                { name: 'Bluetooth' },
                { name: 'Mac Audio' }
            ]
        }
    },
    computed: {
        currentVideoSrc() {
            return this.videos[this.activeButtonIndex]
        }
    },
    mounted() {
        window.addEventListener('scroll', this.handleScroll)
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.handleScroll)
    },
    methods: {
        handleScroll() {
            const container = this.$refs.stickyContainer
            if (!container) return

            const containerRect = container.getBoundingClientRect()
            const containerHeight = container.offsetHeight
            const sectionHeight = window.innerHeight
            const scrollableHeight = containerHeight - sectionHeight

            if (containerRect.top <= 0 && containerRect.bottom > sectionHeight) {
                const scrolled = Math.abs(containerRect.top)
                const progress = Math.min(scrolled / scrollableHeight, 1)
                const newButtonIndex = Math.min(2, Math.floor(progress * 3))

                if (newButtonIndex !== this.activeButtonIndex) {
                    this.setActiveButton(newButtonIndex)
                }
            }
        },

        setActiveButton(index) {
            this.activeButtonIndex = index
        },

        onVideoChange() {
            // S'assurer que la vidéo joue quand elle change
            if (this.$refs.mainVideo) {
                this.$refs.mainVideo.play().catch(() => {
                    // Ignorer les erreurs de lecture automatique
                })
            }
        }
    }
}
</script>

<style scoped>
.sticky-container {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    height: calc(100vh + 1400px);
}

.section-audio-sources {
    position: sticky;
    top: 0;
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    grid-template-rows: auto 1fr;
    background-color: var(--color-background-neutral);
    border-radius: var(--border-radius-large);
    padding: var(--space-08) var(--space-06);
    height: 100vh;
    min-height: 600px;
    z-index: 10;
}

.section-title {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 2 / -2;
    margin-bottom: var(--space-07);
    text-align: center;
}

.section-title__tag {
    grid-column: 2 / -2;
    color: var(--color-brand);
    margin-bottom: var(--space-03);
    display: block;
}

.section-title__title {
    grid-column: 2 / -2;
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
    grid-column: inherit;
    align-items: center;
    justify-content: center;
    gap: var(--space-07);
    max-height: 100%;
    overflow: hidden;
}

.video-container {
    position: relative;
    border-radius: var(--border-radius-xlarge);
    overflow: hidden;
    max-height: 100%;
    aspect-ratio: 7 / 4;
}

.video-element {
    width: 100%;
    height: 100%;
    object-fit: contain;
    display: block;
}

.buttons-container {
    display: flex;
    flex-direction: column;
    gap: var(--space-04);
    align-items: flex-start;
    /* Largeur fixe pour les boutons */
    flex-shrink: 0;
}

.audio-button {
    background: none;
    border: 1px solid var(--color-text);
    color: var(--color-text);
    padding: var(--space-03) var(--space-04);
    border-radius: var(--border-radius-small);
    cursor: pointer;
    transition: opacity 0.3s ease;
    font-family: 'Neue Montreal', sans-serif;
    font-size: var(--font-size-body-medium);
}

/* === RESPONSIVE TABLET === */
@media (max-width: 1024px) {
    .section-title__tag,
    .section-title__title {
        grid-column: 1 / -1;
    }

    .inline-icon {
        width: 24px;
        height: 24px;
    }
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 600px) {
    .section-audio-sources {
        min-height: 500px;
    }

    .audio-sources-display {
        flex-direction: column;
        gap: var(--space-05);
    }

    .video-container {
        aspect-ratio: 4 / 3;
    }

    .buttons-container {
        flex-direction: row;
        justify-content: center;
        width: 100%;
    }

    .inline-icon {
        width: 20px;
        height: 20px;
    }
}
</style>