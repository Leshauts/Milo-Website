<template>
    <section class="section-cover">
        <!-- Logo -->
        <div class="section-cover__logo">
            <img :src="miloLogo" alt="Milo Logo" class="logo-image" />
        </div>

        <!-- Image cover -->
        <div class="section-cover__image">
            <img :src="coverImage" alt="Cover" class="cover-image" />
        </div>

        <!-- SectionTitle utilisé avec size="main" -->
        <SectionTitle 
            :tag="tag"
            :title="title"
            :paragraph="paragraph"
            size="main"
            align="center"
        />

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
import SectionTitle from './SectionTitle.vue'
import spotifyIcon from '../assets/icons/spotify.svg'
import bluetoothIcon from '../assets/icons/bluetooth.svg'
import macosIcon from '../assets/icons/macos.svg'
import powerIcon from '../assets/icons/power.svg'
import multiroomIcon from '../assets/icons/multiroom.svg'
import equalizerIcon from '../assets/icons/equalizer.svg'
import touchIcon from '../assets/icons/touch.svg'
import phoneIcon from '../assets/icons/phone.svg'
import miloLogo from '../assets/images/milo.svg'
import coverImage from '../assets/images/cover.png'

export default {
    name: 'SectionCover',
    components: {
        FeatureItem,
        SectionTitle
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
            phoneIcon,
            miloLogo,
            coverImage
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
    padding: var(--space-05) var(--space-06) var(--space-09) var(--space-06);
    background: linear-gradient(to bottom, #F1F1F1, #DADED9);
}

/* Logo */
.section-cover__logo {
    grid-column: 1 / -1;
    display: flex;
    justify-content: center;
    margin-bottom: var(--space-05);
}

.logo-image {
    height: 40px;
    width: auto;
}

/* Image cover */
.section-cover__image {
    grid-column: 2 / -2;
    display: flex;
    justify-content: center;
    margin-bottom: var(--space-06);
}

.cover-image {
    max-width: 100%;
    height: auto;
    border-radius: var(--border-radius-medium);
}

/* SectionTitle avec marge spécifique à SectionCover */
.section-cover :deep(.section-title) {
    margin-bottom: var(--space-07); /* 48px */
}

/* Grid des features */
.features-grid {
    grid-column: 2 / 8;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: var(--space-02);
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
    .section-cover__image {
        grid-column: 1 / 9;
    }

    .features-grid {
        grid-column: 1 / -1;
        grid-template-columns: repeat(2, 1fr);
    }

    .logo-image {
        height: 32px;
    }
}

@media (max-width: 600px) {
    .section-cover__image {
        grid-column: 1 / 5;
    }
}
</style>