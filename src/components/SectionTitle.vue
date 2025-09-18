<!-- src / components / SectionTitle.vue -->
<template>
  <div class="section-title" :class="[alignClass, variantClass]">
    <div class="section-title__tag body-small">
      {{ tag }}
    </div>
    <h2 class="section-title__title h2">
      {{ title }}
    </h2>
    <p v-if="paragraph" class="section-title__paragraph body-medium">
      {{ paragraph }}
    </p>
  </div>
</template>

<script>
export default {
  name: 'SectionTitle',
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
      default: null
    },
    align: {
      type: String,
      default: 'center',
      validator: (value) => ['left', 'center', 'right'].includes(value)
    },
    variant: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'contrast'].includes(value)
    }
  },
  computed: {
    alignClass() {
      return `section-title--${this.align}`
    },
    variantClass() {
      return `section-title--${this.variant}`
    }
  }
}
</script>

<style scoped>
.section-title {
  grid-column: 3 / -3;
  margin-bottom: var(--space-07);
}

/* === ALIGNEMENT === */
.section-title--left {
  text-align: left;
}

.section-title--center {
  text-align: center;
}

.section-title--right {
  text-align: right;
}

.section-title__tag {
  color: var(--color-brand);
  margin-bottom: var(--space-03);
  display: block;
}

.section-title__title {
  color: var(--color-text);
  margin-bottom: var(--space-05);
}

.section-title__paragraph {
  color: var(--color-text-secondary);
}

/* === VARIANTES DE COULEURS === */
.section-title--contrast .section-title__title {
  color: var(--color-text-contrast);
}

.section-title--contrast .section-title__paragraph {
  color: var(--color-text-light);
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
  .section-title {
    grid-column: 1 / -1;
  }
}
</style>