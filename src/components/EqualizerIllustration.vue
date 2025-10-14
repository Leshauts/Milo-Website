<template>
  <div class="equalizer-illustration">
    <!-- Background image -->
    <img src="/src/assets/images/equalizer/background.png" alt="" class="equalizer-illustration__background" />

    <!-- Title bar -->
    <div class="equalizer-illustration__title-bar">
      <div class="title-bar__left">
        <img src="/src/assets/images/equalizer/icon.png" alt="" class="title-bar__icon" />
        <h5 class="h5 title-bar__title">Equalizer</h5>
      </div>
      <div class="title-bar__toggle">
        <div class="toggle" :class="{ 'toggle--active': isActive }">
          <div class="toggle__knob"></div>
        </div>
      </div>
    </div>

    <!-- Equalizer bands -->
    <div class="equalizer-illustration__bands" ref="bandsWrapper">
      <div class="bands-content" ref="bandsContent" :style="scaleStyle">
        <!-- Frequency Labels -->
        <div class="frequency-labels">
          <div v-for="(band, index) in bands" :key="'label-' + index" class="frequency-label">
            <p class="body-small">{{ band.label }}</p>
          </div>
        </div>

        <!-- Bars Container -->
        <div class="bars-container">
          <RangeSlider v-for="(band, index) in bands" :key="'bar-' + index" v-model="band.value" :min="0" :max="100"
            :step="1" orientation="vertical" :disabled="!isActive" @input="handleBandInput(index, $event)"
            @change="handleBandChange(index, $event)" />
        </div>

        <!-- Values Container -->
        <div class="values-container">
          <p v-for="(band, index) in bands" :key="'value-' + index" class="body-small">
            {{ band.value }}%
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RangeSlider from './RangeSlider.vue'

export default {
  name: 'EqualizerIllustration',
  components: {
    RangeSlider
  },
  data() {
    return {
      isActive: true, // Main toggle state
      bands: [
        { label: '31', value: 85 },
        { label: '63', value: 83 },
        { label: '125', value: 83 },
        { label: '250', value: 74 },
        { label: '500', value: 60 },
        { label: '1k', value: 58 },
        { label: '2k', value: 66 },
        { label: '4k', value: 86 },
        { label: '8k', value: 86 },
        { label: '16k', value: 94 }
      ],
      scaleValue: 1,
      resizeObserver: null
    }
  },
  computed: {
    scaleStyle() {
      return {
        transform: `translate(-50%, -50%) scale(${this.scaleValue})`
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.setupScaling()
    })
  },
  beforeUnmount() {
    if (this.resizeObserver) {
      this.resizeObserver.disconnect()
    }
  },
  methods: {
    handleBandInput(index, value) {
      // Optionnel : faire quelque chose pendant le drag
      console.log(`Band ${this.bands[index].label}: ${value}%`)
    },
    handleBandChange(index, value) {
      // Optionnel : faire quelque chose quand le drag est terminé
      console.log(`Band ${this.bands[index].label} final value: ${value}%`)
    },
    setupScaling() {
      const wrapper = this.$refs.bandsWrapper
      const content = this.$refs.bandsContent

      if (!wrapper || !content) return

      // Dimensions de référence du contenu (taille "idéale" à desktop)
      const contentWidth = 620 // Largeur de référence
      const contentHeight = 400 // Hauteur de référence

      const calculateScale = () => {
        const wrapperRect = wrapper.getBoundingClientRect()

        // Récupérer le padding du wrapper
        const computedStyle = window.getComputedStyle(wrapper)
        const paddingLeft = parseFloat(computedStyle.paddingLeft)
        const paddingRight = parseFloat(computedStyle.paddingRight)
        const paddingTop = parseFloat(computedStyle.paddingTop)
        const paddingBottom = parseFloat(computedStyle.paddingBottom)

        // Calculer l'espace disponible en soustrayant le padding
        const availableWidth = wrapperRect.width - paddingLeft - paddingRight
        const availableHeight = wrapperRect.height - paddingTop - paddingBottom

        // Calculer le scale basé sur le ratio
        const scaleX = availableWidth / contentWidth
        const scaleY = availableHeight / contentHeight

        // Utiliser le plus grand ratio pour remplir tout l'espace (cover behavior)
        this.scaleValue = Math.max(scaleX, scaleY)
      }

      // Initial calculation
      calculateScale()

      // Observer les changements de taille du wrapper
      this.resizeObserver = new ResizeObserver(() => {
        calculateScale()
      })

      this.resizeObserver.observe(wrapper)

      // Aussi écouter le resize de la fenêtre
      window.addEventListener('resize', calculateScale)

      // Cleanup dans beforeUnmount
      this.$once('hook:beforeUnmount', () => {
        window.removeEventListener('resize', calculateScale)
      })
    }
  }
}
</script>

<style scoped>
.equalizer-illustration {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: var(--border-radius-large);
}

/* === BACKGROUND === */
.equalizer-illustration__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* === TITLE BAR === */
.equalizer-illustration__title-bar {
  position: absolute;
  top: var(--space-09);
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-03) var(--space-04) var(--space-03) var(--space-03);
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-large);
  gap: var(--space-05);
  width: calc(100% - (var(--space-08) * 2));
}

.title-bar__left {
  display: flex;
  align-items: center;
  gap: var(--space-03);
}

.title-bar__icon {
  width: 40px;
  height: 40px;
  border-radius: var(--border-radius-small);
}

.title-bar__title {
  margin: 0;
  color: var(--color-text);
}

/* === TOGGLE === */
.toggle {
  width: 56px;
  height: 32px;
  background-color: var(--color-background-strong);
  border-radius: var(--border-radius-full);
  position: relative;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle--active {
  background-color: var(--color-brand);
}

.toggle__knob {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 28px;
  height: 28px;
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-full);
  transition: transform 0.3s ease;
}

.toggle--active .toggle__knob {
  transform: translateX(24px);
}

/* === EQUALIZER BANDS === */
.equalizer-illustration__bands {
  position: absolute;
  top: calc(var(--space-09) + 64px + var(--space-04));
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - (var(--space-08) * 2));
  background-color: var(--color-background-neutral-50);
  border-radius: var(--border-radius-large);
  padding: var(--space-03);
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  /* Aspect ratio: hauteur basée sur la largeur */
  aspect-ratio: 620 / 400;
}

.bands-content {
  position: absolute;
  width: 620px;
  height: 388px;
  padding: 16px;
  background: white;
  border-radius: var(--border-radius-medium);
  top: 50%;
  left: 50%;
  transform-origin: center center;
  display: flex;
  flex-direction: column;
  gap: var(--space-03);
}

/* Frequency Labels */
.frequency-labels {
  display: flex;
  gap: var(--space-04);
  padding-top: var(--space-01);
  justify-content: space-evenly;
  align-items: center;
}

.frequency-label {
  background: var(--color-background-strong);
  border-radius: var(--border-radius-full);
  padding: var(--space-01) 0;
  width: 37px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.frequency-label p {
  color: var(--color-text-secondary);
  text-align: center;
  width: 44px;
}

/* Bars Container */
.bars-container {
  display: flex;
  gap: var(--space-04);
  justify-content: space-evenly;
  align-items: stretch;
  flex: 1;
  min-height: 250px;
}

/* Values Container */
.values-container {
  display: flex;
  gap: var(--space-04);
  justify-content: space-evenly;
  align-items: center;

}

.values-container p {
  color: var(--color-text-secondary);
  text-align: center;
  width: 37px;
  flex-shrink: 0;

}

/* === RESPONSIVE TABLET === */

@media (max-width: 1024px) {
  .equalizer-illustration__bands {
    width: calc(100% - (var(--space-05) * 2));
    /* padding: var(--space-03); */
  }
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 600px) {
  .equalizer-illustration__title-bar {
    padding: var(--space-02) var(--space-03);
    gap: var(--space-04);
  }

  .title-bar__icon {
    width: 32px;
    height: 32px;
  }

  .toggle {
    width: 40px;
    height: 24px;
    border-radius: 12px;
  }

  .toggle__knob {
    width: 20px;
    height: 20px;
  }

  .toggle--active .toggle__knob {
    transform: translateX(16px);
  }
}
</style>
