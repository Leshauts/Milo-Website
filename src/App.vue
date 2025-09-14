<template>
  <GridToggle @toggle="onGridToggle" />
  
  <!-- Section principale avec MainSectionTitle -->
  <Section variant="default">
    <SectionTitle 
      variant="main"
      tag="Système audio moderne"
      title="Milo Audio System"
      paragraph="Transformez votre Raspberry Pi en une plateforme audio polyvalente et moderne. Gérez facilement vos sources audio : Spotify, Bluetooth, MacOS via Snapcast, et bien plus."
    />
  </Section>

  <!-- Section fonctionnalités -->
  <Section variant="strong">
    <SectionTitle 
      tag="Fonctionnalités"
      title="Architecture modulaire pour Raspberry Pi"
      paragraph="Un système pensé pour la flexibilité et la performance, avec une interface utilisateur moderne et responsive."
    />
    
    <div class="features-grid">
      <div class="feature-card" style="grid-column: 1 / 3;">
        <h3 class="h4">Sources multiples</h3>
        <p class="body-medium">Spotify, Bluetooth, AirPlay, et Snapcast pour MacOS</p>
      </div>
      
      <div class="feature-card" style="grid-column: 3 / 6;">
        <h3 class="h4">Interface moderne</h3>
        <p class="body-medium">Design système cohérent avec écran tactile 7"</p>
      </div>
      
      <div class="feature-card" style="grid-column: 6 / -1;">
        <h3 class="h4">Performance optimisée</h3>
        <p class="body-medium">HAT HiFiBerry pour une qualité audio exceptionnelle</p>
      </div>
    </div>
  </Section>

  <!-- Section installation -->
  <Section variant="contrast">
    <SectionTitle 
      tag="Installation"
      title="Mise en route rapide"
    />
    
    <div class="install-commands">
      <pre class="body-mono">npm install milo-audio-system</pre>
      <pre class="body-mono">sudo milo-setup --raspberry-pi</pre>
    </div>
  </Section>
</template>

<script>
import GridToggle from './components/GridToggle.vue'
import Section from './components/Section.vue'
import SectionTitle from './components/SectionTitle.vue'

export default {
  name: 'App',
  components: {
    GridToggle,
    Section,
    SectionTitle
  },
  methods: {
    onGridToggle(isVisible) {
      console.log('Grid visible:', isVisible)
    }
  }
}
</script>

<style>
/* Styles appliqués à l'élément #app (mount point Vue) - SANS scoped */
#app {
  min-height: 100vh;
  background-color: var(--color-background);
  color: var(--color-text);
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  gap: var(--grid-gutter);
  padding: var(--space-08) var(--grid-margin);
  max-width: 100%;
}

.features-grid {
  grid-column: 1 / -1;
  display: contents; /* Pour que les enfants se positionnent directement sur la grille parente */
}

.feature-card {
  background: var(--color-background-neutral);
  padding: var(--space-06);
  border-radius: 16px;
}

.feature-card h3 {
  color: var(--color-text);
  margin-bottom: var(--space-03);
}

.feature-card p {
  color: var(--color-text-secondary);
}

.install-commands {
  grid-column: 3 / -3; /* Centré avec marges */
}

.install-commands pre {
  background: rgba(255, 255, 255, 0.1);
  padding: var(--space-04);
  border-radius: 8px;
  margin-bottom: var(--space-03);
  color: var(--color-text-contrast);
}

@media (max-width: 768px) {
  #app {
    padding: var(--space-06) var(--grid-margin);
  }
  
  .feature-card {
    grid-column: 1 / -1 !important; /* Force sur toute la largeur en mobile */
  }
  
  .install-commands {
    grid-column: 1 / -1; /* Pleine largeur en mobile */
  }
}
</style>