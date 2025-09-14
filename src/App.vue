<template>
  <GridToggle @toggle="onGridToggle" />
  <section class="demo-section">
    <h1 class="h1">Milo Audio System</h1>
    <h2 class="h2">Architecture modulaire pour Raspberry Pi</h2>
    <h3 class="h3">Sources audio multiples</h3>
    <h4 class="h4">Interface moderne</h4>
    <h5 class="h5">Performance optimisée</h5>

    <p class="body-large">
      Milo transforme votre Raspberry Pi en une plateforme audio polyvalente et moderne.
    </p>

    <p class="body-medium">
      Gérez facilement vos sources audio : Spotify, Bluetooth, MacOS via Snapcast, et bien plus.
    </p>

    <p class="body-small">
      Design système cohérent et responsive pour une expérience utilisateur optimale.
    </p>

    <p class="body-mono">
      npm install milo-audio-system
    </p>
  </section>
</template>

<script>
import GridToggle from './components/GridToggle.vue'

export default {
  name: 'App',
  components: {
    GridToggle
  },
  methods: {
    onGridToggle(isVisible) {
      console.log('Grid visible:', isVisible)
    }
  }
}
</script>

<style scoped>
#app {
  min-height: 100vh;
  background-color: var(--color-background);
  color: var(--color-text);
  /* === GRID === */
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  gap: var(--grid-gutter);
  padding: 0 var(--grid-margin);
  max-width: 100%;
}

.demo-section {
  max-width: 800px;
  margin: 0 auto;
  padding: var(--space-08) var(--space-06);
}

.demo-section>* {
  margin-bottom: var(--space-05);
}
</style>