<template>
  <div :class="['slider-container', orientation]" :style="cssVars">
    <input
      type="range"
      :class="['range-slider', orientation]"
      :min="min"
      :max="max"
      :step="step"
      :value="modelValue"
      @input="handleInput"
      @change="handleChange"
      @pointerdown="handlePointerDown"
      @pointerup="handlePointerUp"
      :disabled="disabled"
    />

    <div
      v-if="orientation === 'horizontal' && !hideInlineValue"
      class="slider-value text-mono"
      :class="{ dragging: isDragging }"
    >
      {{ modelValue }}{{ valueUnit }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'RangeSlider',
  props: {
    modelValue: { type: Number, required: true },
    min: { type: Number, default: 0 },
    max: { type: Number, default: 100 },
    step: { type: Number, default: 1 },
    orientation: { type: String, default: 'horizontal' },
    disabled: { type: Boolean, default: false },
    valueUnit: { type: String, default: '' },
    hideInlineValue: { type: Boolean, default: false }
  },
  emits: ['update:modelValue', 'input', 'change', 'drag-start', 'drag-end'],
  data() {
    return {
      isDragging: false
    }
  },
  computed: {
    percentage() {
      const rawPercentage = ((this.modelValue - this.min) / (this.max - this.min)) * 100

      if (this.orientation === 'horizontal') {
        const thumbAdjustment = 15
        return rawPercentage * (100 - thumbAdjustment) / 100 + thumbAdjustment / 2
      } else {
        const thumbAdjustment = 11
        return rawPercentage * (100 - thumbAdjustment) / 100 + thumbAdjustment / 2
      }
    },
    cssVars() {
      return {
        '--progress': `${this.percentage}%`
      }
    }
  },
  methods: {
    handleInput(event) {
      const value = parseInt(event.target.value)
      this.$emit('input', value)
      this.$emit('update:modelValue', value)
    },
    handleChange(event) {
      const value = parseInt(event.target.value)
      this.$emit('change', value)
      this.$emit('update:modelValue', value)
    },
    handlePointerDown(event) {
      if (event.button !== 0 || this.disabled) return
      this.isDragging = true
      this.$emit('drag-start')
    },
    handlePointerUp() {
      if (this.isDragging) {
        this.isDragging = false
        this.$emit('drag-end')
      }
    }
  }
}
</script>

<style scoped>
.slider-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.slider-container.horizontal {
  width: 100%;
  height: 40px;
}

.slider-container.vertical {
  width: 40px;
  flex: 1;
  flex-direction: column;
}

.range-slider {
  -webkit-appearance: none;
  appearance: none;
  outline: none;
  cursor: pointer;
  border: none;
  border-radius: 20px;
  transition: opacity 300ms ease;
  pointer-events: none;
}

.range-slider.horizontal {
  width: 100%;
  height: 40px;
  background: linear-gradient(to right,
      var(--color-text-secondary) 0%,
      var(--color-text-secondary) var(--progress),
      var(--color-background-strong) var(--progress),
      var(--color-background-strong) 100%);
}

.range-slider.vertical {
  width: 40px;
  min-height: 260px;
  flex: 1;
  writing-mode: vertical-lr;
  direction: rtl;
  background: linear-gradient(to top,
      var(--color-text-secondary) 0%,
      var(--color-text-secondary) var(--progress),
      var(--color-background-strong) var(--progress),
      var(--color-background-strong) 100%);
}

.range-slider.horizontal::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 62px;
  height: 40px;
  border-radius: 20px;
  background: var(--color-background-neutral);
  border: 2px solid var(--color-text-secondary);
  cursor: pointer;
  box-shadow: none;
  pointer-events: auto;
}

.range-slider.horizontal::-moz-range-thumb {
  width: 58px;
  height: 36px;
  border-radius: 20px;
  background: var(--color-background-neutral);
  border: 2px solid var(--color-text-secondary);
  cursor: pointer;
  pointer-events: auto;
}

.range-slider.vertical::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 40px;
  height: 62px;
  border-radius: 20px;
  background: var(--color-background-neutral);
  border: 2px solid var(--color-text-secondary);
  cursor: pointer;
  pointer-events: auto;
}

.range-slider.vertical::-moz-range-thumb {
  width: 36px;
  height: 58px;
  border-radius: 20px;
  background: var(--color-background-neutral);
  border: 2px solid var(--color-text-secondary);
  cursor: pointer;
  pointer-events: auto;
}

.range-slider::-webkit-slider-track {
  background: transparent;
  border: none;
  pointer-events: none;
}

.range-slider::-moz-range-track {
  background: transparent;
  border: none;
  pointer-events: none;
}

.range-slider:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.range-slider:disabled::-webkit-slider-thumb,
.range-slider:disabled::-moz-range-thumb {
  cursor: not-allowed;
}

.slider-value {
  position: absolute;
  right: var(--space-04);
  color: var(--color-text-secondary);
  transition: color 300ms ease;
  pointer-events: none;
  z-index: 1;
}

.slider-value.dragging {
  color: var(--color-brand);
}
</style>
