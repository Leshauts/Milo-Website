<template>
    <section class="section-cover">
        <!-- SectionTitle main intégré -->
        <div class="section-title-main">
            <div class="section-title-main__tag body-large">
                {{ tag }}
            </div>
            <h1 class="section-title-main__title h1">
                {{ title }}
            </h1>
            <p class="section-title-main__paragraph body-large">
                {{ paragraph }}
            </p>
        </div>

        <!-- Grid des 8 features -->
        <div class="features-grid">
            <FeatureItem name="Spotify" :icon="spotifyIcon" />
            <FeatureItem name="Bluetooth" :icon="bluetoothIcon" />
            <FeatureItem name="Mac Audio" :icon="macosIcon" />
            <FeatureItem name="60 Watts Amp" :icon="powerIcon" />
            <FeatureItem name="Multiroom" :icon="multiroomIcon" />
            <FeatureItem name="Audio EQ" :icon="equalizerIcon" />
            <FeatureItem name="Touch display" :icon="touchIcon" />
            <FeatureItem name="Remote control" :icon="phoneIcon" />
        </div>
    </section>
</template>

<script>
import FeatureItem from './FeatureItem.vue'
import spotifyIcon from '../assets/icons/spotify.svg'
import bluetoothIcon from '../assets/icons/bluetooth.svg'
import macosIcon from '../assets/icons/macos.svg'
import powerIcon from '../assets/icons/power.svg'
import multiroomIcon from '../assets/icons/multiroom.svg'
import equalizerIcon from '../assets/icons/equalizer.svg'
import touchIcon from '../assets/icons/touch.svg'
import phoneIcon from '../assets/icons/phone.svg'

export default {
    name: 'SectionCover',
    components: {
        FeatureItem
    },
    props: {
        tag: {
            type: String,
            required: true
        },
        title: {
            type: String,
            required: true
        },
        paragraph: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            spotifyIcon,
            bluetoothIcon,
            macosIcon,
            powerIcon,
            multiroomIcon,
            equalizerIcon,
            touchIcon,
            phoneIcon
        }
    }
}
</script>

<style scoped>
.section-cover {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 1 / -1;
    border-radius: var(--border-radius-large);
    padding: var(--space-09) var(--space-06);
    background: linear-gradient(to bottom, #F1F1F1, #DADED9);
}

/* SectionTitle main intégré */
.section-title-main {
    grid-column: 3 / -3;
    margin-bottom: var(--space-07);
    text-align: center;
}

.section-title-main__tag {
    color: var(--color-brand);
    margin-bottom: var(--space-03);
    display: block;
}

.section-title-main__title {
    color: var(--color-text);
    margin-bottom: var(--space-05);
}

.section-title-main__paragraph {
    color: var(--color-text-secondary);
}

/* Grid des features */
.features-grid {
    grid-column: 2 / 8;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: var(--space-03);
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
    .section-title-main {
        grid-column: 1 / -1;
    }

    .features-grid {
        grid-column: 1 / -1;
        grid-template-columns: repeat(2, 1fr);
    }
}
</style>