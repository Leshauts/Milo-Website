<template>
  <div class="equalizer-illustration">
    <!-- Background image -->
    <img src="/src/assets/images/equalizer/background.png" alt="" class="equalizer-illustration__background" />

    <!-- Content wrapper (title bar + bands) -->
    <div class="equalizer-illustration__content">
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
      <div class="equalizer-illustration__bands-container" ref="bandsContainer">
        <div class="equalizer-illustration__bands" :style="scaleStyle">
          <div class="bands-content">
            <!-- Frequency Labels -->
            <div class="frequency-labels">
              <div v-for="(band, index) in bands" :key="'label-' + index" class="frequency-label">
                <p class="body-mono">{{ band.label }}</p>
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
              <p v-for="(band, index) in bands" :key="'value-' + index" class="body-mono">
                {{ band.value }}%
              </p>
            </div>
          </div>
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
      resizeObserver: null,
      resizeHandler: null
    }
  },
  computed: {
    scaleStyle() {
      return {
        transform: `scale(${this.scaleValue})`
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
    if (this.resizeHandler) {
      window.removeEventListener('resize', this.resizeHandler)
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
      const container = this.$refs.bandsContainer

      if (!container) return

      // Dimensions de référence du bands (taille "idéale" à desktop)
      const referenceWidth = 620 // Largeur de référence
      const referenceHeight = 400 // Hauteur de référence

      const calculateScale = () => {
        const containerRect = container.getBoundingClientRect()

        // Calculer le scale basé sur le ratio
        const scaleX = containerRect.width / referenceWidth
        const scaleY = containerRect.height / referenceHeight

        // Utiliser le plus petit ratio pour que tout rentre (contain behavior)
        this.scaleValue = Math.min(scaleX, scaleY)
      }

      // Initial calculation
      calculateScale()

      // Observer les changements de taille du container
      this.resizeObserver = new ResizeObserver(() => {
        calculateScale()
      })

      this.resizeObserver.observe(container)

      // Stocker le handler pour pouvoir le supprimer dans beforeUnmount
      this.resizeHandler = calculateScale
      window.addEventListener('resize', this.resizeHandler)
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
  border-radius: var(--border-radius-xxlarge);
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

/* === CONTENT WRAPPER === */
.equalizer-illustration__content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: calc(100% - (var(--space-08) * 2));
  display: flex;
  flex-direction: column;
  gap: var(--space-04);
}

/* === TITLE BAR === */
.equalizer-illustration__title-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-03) var(--space-04) var(--space-03) var(--space-03);
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-large);
  gap: var(--space-05);
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
/* Container pour le positionnement */
.equalizer-illustration__bands-container {
  width: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  aspect-ratio: 620 / 400;
}

/* Bands scalé avec border-radius */
.equalizer-illustration__bands {
  width: 620px;
  height: 400px;
  aspect-ratio: 620 / 400;
  background-color: var(--color-background-neutral-50);
  border-radius: var(--border-radius-xlarge);
  padding: var(--space-03);
  transform-origin: center center;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Contenu qui prend 100% */
.bands-content {
  width: 100%;
  height: 100%;
  padding: 16px;
  background: white;
  border-radius: var(--border-radius-small);
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
  .equalizer-illustration__content {
    width: calc(100% - (var(--space-07) * 2));
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
