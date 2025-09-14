<template>
  <section class="section" :class="sectionClass">
    <div class="section-content">
      <slot />
    </div>
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
  grid-column: 1 / -1;
  border-radius: 24px;
  padding: var(--space-08) var(--space-06);
  margin-bottom: var(--space-06);
  display: contents;
}

.section--default {
  background-color: var(--color-background-neutral);
}


.section--contrast {
  background-color: var(--color-background-contrast);
  color: var(--color-text-contrast);
}

.section-content {
  display: contents;
}

</style>