<template>
  <div class="section-title">
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
    }
  }
}
</script>

<style scoped>
.section-title {
  grid-column: 2 / -2;
  margin-bottom: var(--space-07);
  text-align: center;
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

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
  .section-title {
    grid-column: 1 / -1;
  }
}
</style>