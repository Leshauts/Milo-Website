<template>
    <section class="section-audio-sources">
        <div class="section-title">
            <div class="section-title__tag body-small">
                Audio Sources
            </div>
            <h2 class="section-title__title h2">
                Play audio from <span class="no-wrap"><img :src="spotifyIcon" alt="Spotify" class="inline-icon" />Spotify</span>, <span class="no-wrap"><img :src="bluetoothIcon" alt="Bluetooth" class="inline-icon" />Bluetooth</span> or&nbsp;your <span class="no-wrap"><img :src="macosIcon" alt="Mac" class="inline-icon" />Mac</span>.
            </h2>
        </div>

        <div class="audio-sources-display">
            <div class="audio-source-item">
                Element 1
            </div>
            <div class="audio-source-item">
                Element 2
            </div>
        </div>
    </section>
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
            macosIcon
        }
    }
}
</script>

<style scoped>
.section-audio-sources {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    background-color: var(--color-background-neutral);
    border-radius: var(--border-radius-large);
    padding: var(--space-07) var(--space-06);
}

.section-title {
    grid-column: 3 / -3;
    margin-bottom: var(--space-07);
    text-align: center;
}

.section-title__tag {
    color: var(--color-brand);
    margin-bottom: var(--space-03);
    display: block;
}

.section-title__title {
    color: var(--color-text);
}

.inline-icon {
    width: 32px;
    height: 32px;
    vertical-align: middle;
    margin-right: var(--space-02);
    margin-left: var(--space-02);
    margin-bottom: var(--space-01);

}
.no-wrap {
  white-space: nowrap;
}

.audio-sources-display {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
}

.audio-source-item:nth-child(1) {
    grid-column: 2 / 5;
}

.audio-source-item:nth-child(2) {
    grid-column: 5 / 7;
}

/* === RESPONSIVE TABLET === */
@media (max-width: 1024px) {
    .section-title {
        grid-column: 1 / -1;
    }

    .inline-icon {
        width: 24px;
        height: 24px;
    }
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 600px) {
    .inline-icon {
        width: 20px;
        height: 20px;
    }
}
</style>