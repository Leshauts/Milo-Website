<template>
  <section class="section-cover">
    <!-- SectionTitle main intégré -->
    <div class="section-title-main">
      <div class="section-title-main__tag body-large">
        {{ tag }}
      </div>
      <h1 class="section-title-main__title h1">
        {{ title }}
      </h1>
      <p class="section-title-main__paragraph body-large">
        {{ paragraph }}
      </p>
    </div>
    
    <!-- Grid des 8 features -->
    <div class="features-grid">
      <div class="feature">Feature 1</div>
      <div class="feature">Feature 2</div>
      <div class="feature">Feature 3</div>
      <div class="feature">Feature 4</div>
      <div class="feature">Feature 5</div>
      <div class="feature">Feature 6</div>
      <div class="feature">Feature 7</div>
      <div class="feature">Feature 8</div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'SectionCover',
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
      required: true
    }
  }
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

/* SectionTitle main intégré */
.section-title-main {
  grid-column: 3 / -3;
  margin-bottom: var(--space-06);
  text-align: center;
}

.section-title-main__tag {
  color: var(--color-brand);
  margin-bottom: var(--space-03);
  display: block;
}

.section-title-main__title {
  color: var(--color-text);
  margin-bottom: var(--space-05);
}

.section-title-main__paragraph {
  color: var(--color-text-secondary);
}

/* Grid des features */
.features-grid {
  grid-column: 2 / 8;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-04);
}

.feature {
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-small);
  padding: var(--space-04);
  text-align: center;
  font-family: 'Neue Montreal', sans-serif;
  font-size: var(--font-size-body-small);
  color: var(--color-text);
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 768px) {
  .section-title-main {
    grid-column: 1 / -1;
  }

  .features-grid {
    grid-column: 1 / -1;
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>