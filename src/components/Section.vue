<template>
  <section class="section" :class="sectionClass">
    <slot />
  </section>
</template>

<script>
export default {
  name: 'Section',
  props: {
    variant: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'contrast'].includes(value)
    }
  },
  computed: {
    sectionClass() {
      return `section--${this.variant}`
    }
  }
}
</script>

<style scoped>
.section {
  display: grid;
  grid-template-columns: subgrid;
  grid-column: 1 / -1;
  border-radius: var(--border-radius-large);
  /* padding: var(--space-08) var(--space-06); */
}

.section--default {
  background-color: var(--color-background-neutral);
}

.section--contrast {
  background-color: var(--color-background-contrast);
  color: var(--color-text-contrast);
}
</style>