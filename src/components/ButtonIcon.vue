<!-- src / components / ButtonIcon.vue -->
<template>
    <button class="button-icon" @click="$emit('click')">
        <img :src="iconSrc" :alt="iconAlt" class="button-icon__image" />
    </button>
</template>

<script>
import bluetoothIcon from '../assets/icons/bluetooth.svg'
import equalizerIcon from '../assets/icons/equalizer.svg'
import macosIcon from '../assets/icons/macos.svg'
import multiroomIcon from '../assets/icons/multiroom.svg'
import phoneIcon from '../assets/icons/phone.svg'
import powerIcon from '../assets/icons/power.svg'
import spotifyIcon from '../assets/icons/spotify.svg'
import touchIcon from '../assets/icons/touch.svg'
import fullscreenIcon from '../assets/icons/fullscreen.svg'
import pauseIcon from '../assets/icons/pause.svg'
import playIcon from '../assets/icons/play.svg'

export default {
    name: 'ButtonIcon',
    emits: ['click'],
    props: {
        icon: {
            type: String,
            required: true,
            validator(value) {
                return [
                    'bluetooth',
                    'equalizer', 
                    'macos',
                    'multiroom',
                    'phone',
                    'power',
                    'spotify',
                    'touch',
                    'fullscreen',
                    'pause',
                    'play'
                ].includes(value)
            }
        }
    },
    computed: {
        iconSrc() {
            const iconMap = {
                bluetooth: bluetoothIcon,
                equalizer: equalizerIcon,
                macos: macosIcon,
                multiroom: multiroomIcon,
                phone: phoneIcon,
                power: powerIcon,
                spotify: spotifyIcon,
                touch: touchIcon,
                fullscreen: fullscreenIcon,
                pause: pauseIcon,
                play: playIcon
            }
            return iconMap[this.icon]
        },
        iconAlt() {
            return `${this.icon} icon`
        }
    }
}
</script>

<style scoped>
.button-icon {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: var(--space-03);
    background-color: var(--color-background-neutral-12);
    border: none;
    border-radius: var(--border-radius-small);
    cursor: pointer;
    min-width: 48px;
    min-height: 48px;
}
.button-icon:hover {
    background-color: var(--color-background-neutral-24);
}

.button-icon__image {
    width: 24px;
    height: 24px;
    display: block;
}
</style>