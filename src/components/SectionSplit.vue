<template>
  <section class="section-split" :class="sectionClass">
    <!-- Contenu texte -->
    <div class="section-split__content" :class="contentClass">
      <slot name="content" />
    </div>
    
    <!-- Bloc illustration (carré) -->
    <div class="section-split__illustration" :class="illustrationClass">
      <slot name="illustration">
        <!-- Placeholder vide pour l'illustration -->
        <div class="illustration-placeholder"></div>
      </slot>
    </div>
  </section>
</template>

<script>
export default {
  name: 'SectionSplit',
  props: {
    variant: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'contrast'].includes(value)
    },
    reversed: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    sectionClass() {
      return `section-split--${this.variant}`
    },
    contentClass() {
      return {
        'section-split__content--reversed': this.reversed
      }
    },
    illustrationClass() {
      return {
        'section-split__illustration--reversed': this.reversed
      }
    }
  }
}
</script>

<style scoped>
.section-split {
  grid-column: 1 / -1;
  border-radius: var(--border-radius-large);
  display: flex;
  align-items: center;
  gap: var(--grid-gutter);
}

.section-split--default {
  background-color: var(--color-background-neutral);
}

.section-split--contrast {
  background-color: var(--color-background-contrast);
  color: var(--color-text-contrast);
}

/* === CONTENU TEXTE === */
.section-split__content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.section-split__content--reversed {
  order: 2;
}

/* === ILLUSTRATION === */
.section-split__illustration {
  flex: 1;
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.section-split__illustration--reversed {
  order: 1;
}

/* Placeholder pour l'illustration */
.illustration-placeholder {
  width: 100%;
  height: 100%;
  background: rgba(239, 100, 46, 0.1);
  border: 2px dashed rgba(239, 100, 46, 0.3);
  border-radius: var(--border-radius-medium);
  display: flex;
  align-items: center;
  justify-content: center;
}

.illustration-placeholder::after {
  content: 'Illustration';
  font-family: 'Space Mono', monospace;
  font-size: var(--font-size-body-small);
  color: var(--color-brand);
  opacity: 0.6;
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 768px) {
  .section-split {
    flex-direction: column;
    gap: var(--space-06);
    min-height: auto;
  }

  .section-split__content,
  .section-split__content--reversed {
    order: 1 !important; /* Toujours en premier en mobile */
  }

  .section-split__illustration,
  .section-split__illustration--reversed {
    order: 2 !important; /* Toujours en second en mobile */
    max-width: 280px;
  }
}
</style>