<template>
  <img :src="iconSrc" :alt="iconAlt" :style="iconStyle" class="icon" />
</template>

<script>
import playIcon from '../assets/icons/play.svg'
import pauseIcon from '../assets/icons/pause.svg'
import previousIcon from '../assets/icons/previous.svg'
import nextIcon from '../assets/icons/next.svg'

export default {
  name: 'Icon',
  props: {
    name: {
      type: String,
      required: true,
      validator(value) {
        return ['play', 'pause', 'previous', 'next'].includes(value)
      }
    },
    size: {
      type: [Number, String],
      default: 24
    }
  },
  computed: {
    iconSrc() {
      const iconMap = {
        play: playIcon,
        pause: pauseIcon,
        previous: previousIcon,
        next: nextIcon
      }
      return iconMap[this.name]
    },
    iconAlt() {
      return `${this.name} icon`
    },
    iconStyle() {
      const size = typeof this.size === 'number' ? `${this.size}px` : this.size
      return {
        width: size,
        height: size
      }
    }
  }
}
</script>

<style scoped>
.icon {
  display: block;
  pointer-events: none;
}
</style>
