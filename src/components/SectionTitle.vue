<!-- src / components / SectionTitle.vue -->
<template>
  <div class="section-title" :class="[alignClass, variantClass, sizeClass]">
    <div class="section-title__tag" :class="tagClass">
      {{ tag }}
    </div>
    
    <!-- Titre avec slot ou texte simple -->
    <component 
      :is="titleTag" 
      class="section-title__title" 
      :class="titleClass"
      v-if="!$slots.title"
    >
      {{ title }}
    </component>
    
    <component 
      :is="titleTag" 
      class="section-title__title" 
      :class="titleClass"
      v-else
    >
      <slot name="title" />
    </component>
    
    <p v-if="paragraph" class="section-title__paragraph" :class="paragraphClass">
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
      default: null
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
    },
    size: {
      type: String,
      default: 'section',
      validator: (value) => ['main', 'section'].includes(value)
    }
  },
  computed: {
    alignClass() {
      return `section-title--${this.align}`
    },
    variantClass() {
      return `section-title--${this.variant}`
    },
    sizeClass() {
      return `section-title--${this.size}`
    },
    titleTag() {
      return this.size === 'main' ? 'h1' : 'h2'
    },
    titleClass() {
      return this.size === 'main' ? 'h1' : 'h2'
    },
    tagClass() {
      return this.size === 'main' ? 'body-large' : 'body-small'
    },
    paragraphClass() {
      return this.size === 'main' ? 'body-large' : 'body-medium'
    }
  }
}
</script>

<style scoped>
.section-title {
  grid-column: 3 / -3;
  z-index: 99;
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
  /* Pas de margin-bottom par défaut - géré par les composants parents */
}

.section-title__paragraph {
  color: var(--color-text-secondary);
  margin-top: var(--space-05);
}

/* === VARIANTES DE COULEURS === */
.section-title--contrast .section-title__title {
  color: var(--color-text-contrast);
}

.section-title--contrast .section-title__paragraph {
  color: var(--color-text-light);
}

/* === VARIANTES DE TAILLES === */
.section-title--main .section-title__tag {
  margin-bottom: var(--space-04);
}

.section-title--section .section-title__tag {
  margin-bottom: var(--space-03);
}

/* Styles spécifiques pour les icônes inline dans les titres */
.section-title__title .inline-icon {
  width: 32px;
  height: 32px;
  vertical-align: middle;
  margin-right: var(--space-02);
}

.section-title__title .no-wrap {
  white-space: nowrap;
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
  .section-title {
    grid-column: 2 / -2;
  }
  
  .section-title__title .inline-icon {
    width: 24px;
    height: 24px;
  }
}

@media (max-width: 600px) {
  .section-title__title .inline-icon {
    width: 20px;
    height: 20px;
  }
    .section-title {
    grid-column: 1 / -1;
  }
}
</style>