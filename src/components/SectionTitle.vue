<template>
  <div class="section-title" :class="{ 'section-title--main': isMain }">
    <div class="section-title__tag" :class="tagClass">
      {{ tag }}
    </div>
    <h1 v-if="isMain" class="section-title__title h1">
      {{ title }}
    </h1>
    <h2 v-else class="section-title__title h2">
      {{ title }}
    </h2>
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
      required: true
    },
    paragraph: {
      type: String,
      default: null
    },
    variant: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'main'].includes(value)
    }
  },
  computed: {
    isMain() {
      return this.variant === 'main'
    },
    tagClass() {
      return this.isMain ? 'body-large' : 'body-small'
    },
    paragraphClass() {
      return this.isMain ? 'body-large' : 'body-medium'
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
.section-title--main {
  grid-column: 3 / -3;
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


</style>