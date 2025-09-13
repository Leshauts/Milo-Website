<template>
  <div id="app">
    <!-- Test des styles typographiques -->
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
  </div>
</template>

<script setup>
// Logique du composant ici
</script>

<style scoped>
#app {
  min-height: 100vh;
  background-color: var(--color-background);
  color: var(--color-text);
}

.demo-section {
  max-width: 800px;
  margin: 0 auto;
  padding: var(--space-08) var(--space-06);
}

.demo-section > * {
  margin-bottom: var(--space-05);
}
</style>