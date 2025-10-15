<template>
  <section class="section-split" :class="[sectionClass, variantClass]">
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
    reversed: {
      type: Boolean,
      default: false
    },
    variant: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'contrast'].includes(value)
    }
  },
  computed: {
    sectionClass() {
      return this.reversed ? 'section-split--reversed' : 'section-split--default'
    },
    variantClass() {
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
  /* Configuration de la section avec grille */
  display: grid;
  grid-template-columns: subgrid;
  grid-column: 1 / -1;
  border-radius: var(--border-radius-xxlarge);

  /* Configuration du contenu split */
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: var(--grid-gutter);
}

/* === VARIANTES DE SECTION === */
.section-split--default {
  background-color: var(--color-background-neutral);
  color: var(--color-text);
}

.section-split--contrast {
  background-color: var(--color-background-contrast);
  color: var(--color-text-contrast);
}

/* === CONTENU TEXTE === */
.section-split__content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 var(--space-07) 0 var(--space-09);
}

.section-split__content--reversed {
  order: 2;
  padding: 0 var(--space-08) 0 calc(var(--space-08) + 16px);
}



/* === ILLUSTRATION === */
.section-split__illustration {
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

/* === RESPONSIVE TABLET === */
@media (max-width: 1024px) {


  .section-split__content,
  .section-split__content--reversed {
    padding: 0 var(--space-06) 0 var(--space-06);
  }

  .section-split__illustration {
  aspect-ratio: 9 / 10;
}

}

/* === RESPONSIVE MOBILE === */
@media (max-width: 820px) {
  .section-split {
    grid-template-columns: 1fr;
    gap: 0;
  }

  .section-split__content,
  .section-split__content--reversed {
    padding: var(--space-09) var(--space-06) var(--space-09) var(--space-06);
  }
  .section-split__illustration {
  aspect-ratio: 1;
}
  .section-split__illustration,
  .section-split__illustration--reversed {
    order: 2 !important;
    width: 100%;
    justify-self: center;
  }
}
</style>