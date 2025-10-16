<template>
  <div class="app-icon" :style="iconStyle">
    <img :src="iconSrc" :alt="iconAlt" class="app-icon__image" />
  </div>
</template>

<script>
import spotifyIcon from '../assets/icons/spotify-color.svg'
import bluetoothIcon from '../assets/icons/bluetooth-color.svg'
import macosIcon from '../assets/icons/macos-color.svg'
import multiroomIcon from '../assets/icons/multiroom.svg'
import equalizerIcon from '../assets/icons/equalizer.svg'

export default {
  name: 'AppIcon',
  props: {
    name: {
      type: String,
      required: true,
      validator(value) {
        return ['librespot', 'bluetooth', 'roc', 'multiroom', 'equalizer'].includes(value)
      }
    },
    size: {
      type: [Number, String],
      default: 32
    }
  },
  computed: {
    iconSrc() {
      const iconMap = {
        librespot: spotifyIcon,
        bluetooth: bluetoothIcon,
        roc: macosIcon,
        multiroom: multiroomIcon,
        equalizer: equalizerIcon
      }
      return iconMap[this.name]
    },
    iconAlt() {
      return `${this.name} icon`
    },
    // iconStyle() {
    //   let sizeInPx = 32

    //   if (typeof this.size === 'number') {
    //     sizeInPx = this.size
    //   } else if (typeof this.size === 'string') {
    //     switch (this.size) {
    //       case 'large': sizeInPx = 72; break
    //       case 'medium': sizeInPx = 64; break
    //       case 'small': sizeInPx = 32; break
    //       default: sizeInPx = 32
    //     }
    //   }

    //   return {
    //     width: `${sizeInPx}px`,
    //     height: `${sizeInPx}px`
    //   }
    // }
  }
}
</script>

<style scoped>
.app-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  border-radius: var(--radius-02);
  overflow: hidden;
}

.app-icon__image {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

/* @media (max-aspect-ratio: 4/3) {
  .app-icon[style*="72px"] {
    width: 64px !important;
    height: 64px !important;
  }
} */
</style>
