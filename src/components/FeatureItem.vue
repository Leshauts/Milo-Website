<template>
  <div class="feature-item">
    <!-- Ligne : icon + name -->
    <div class="feature-item__header">
      <div class="feature-item__icon">
        <!-- Placeholder icon 24x24 -->
        <div class="icon-placeholder"></div>
      </div>
      <div class="feature-item__name body-medium">
        {{ name }}
      </div>
    </div>
    
    <!-- Paragraphe descriptif -->
    <p class="feature-item__paragraph body-small">
      {{ paragraph }}
    </p>
  </div>
</template>

<script>
export default {
  name: 'FeatureItem',
  props: {
    name: {
      type: String,
      required: true
    },
    paragraph: {
      type: String,
      default: "Milō in Spotify to enjoy your favorite music & playlists."
    }
  }
}
</script>

<style scoped>
.feature-item {
  background-color: var(--color-background-strong);
  border-radius: var(--border-radius-small);
  padding: var(--space-04);
}

.feature-item__header {
  display: flex;
  align-items: center;
  gap: var(--space-02);
  margin-bottom: var(--space-02);
}

.feature-item__icon {
  width: 24px;
  height: 24px;
  flex-shrink: 0;
}

.icon-placeholder {
  width: 100%;
  height: 100%;
  background-color: var(--color-brand);
  border-radius: var(--border-radius-small);
}

.feature-item__name {
  color: var(--color-text);
  font-weight: 500;
}

.feature-item__paragraph {
  color: var(--color-text-secondary);
  line-height: var(--line-height-body-small);
}
</style>