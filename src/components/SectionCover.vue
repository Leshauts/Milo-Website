<template>
  <section class="section-cover">
    <slot />
  </section>
</template>

<script>
export default {
  name: 'SectionCover'
}
</script>

<style scoped>
.section-cover {
  display: grid;
  grid-template-columns: subgrid;
  grid-column: 1 / -1;
  border-radius: var(--border-radius-large);
  padding: var(--space-08) var(--space-06);
  margin-bottom: var(--space-06);
  background: linear-gradient(to bottom, #F1F1F1, #D1DED9);
}

/* Positionnement des SectionTitle enfants */
.section-cover :deep(.section-title) {
  grid-column: 2 / -2;
}

.section-cover :deep(.section-title--main) {
  grid-column: 3 / -3;
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 768px) {
  .section-cover :deep(.section-title),
  .section-cover :deep(.section-title--main) {
    grid-column: 1 / -1;
  }
}
</style>