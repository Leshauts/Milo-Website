<template>
  <div id="app">
    <!-- Exemple d'utilisation de la grille -->
    <div class="grid-container">
      <!-- Titre principal sur colonnes 1-5 -->
      <div class="hero-section">
        <h1 class="h1">Mon super projet</h1>
        <p class="body-large">Description du projet sur plusieurs colonnes</p>
      </div>
      
      <!-- Sidebar sur colonnes 6-8 -->
      <div class="sidebar">
        <h3 class="h3">Navigation</h3>
        <p class="body-medium">Menu de navigation</p>
      </div>
      
      <!-- Contenu principal pleine largeur -->
      <div class="main-content">
        <h2 class="h2">Contenu principal</h2>
        <p class="body-medium">Lorem ipsum dolor sit amet...</p>
      </div>
      
      <!-- Deux cartes côte à côte -->
      <div class="card">
        <h4 class="h4">Carte 1</h4>
        <p class="body-small">Contenu de la carte</p>
      </div>
      
      <div class="card">
        <h4 class="h4">Carte 2</h4>
        <p class="body-small">Contenu de la carte</p>
      </div>
    </div>
    
    <!-- Composant toggle -->
    <GridToggle @toggle="onGridToggle" />
  </div>
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

<style>
/* Importer le design system */
@import './assets/styles/design-system.css';

/* Styles spécifiques au layout */
#app {
  min-height: 100vh;
  background: var(--color-background);
}

/* === POSITIONNEMENT SUR LA GRILLE === */

/* Hero section : colonnes 1-5 (desktop) / pleine largeur (mobile) */
.hero-section {
  grid-column: 1 / 6;
}

/* Sidebar : colonnes 6-8 (desktop) / pleine largeur (mobile) */
.sidebar {
  grid-column: 6 / 9;
  background: var(--color-background-neutral);
  padding: var(--space-04);
  border-radius: 8px;
}

/* Contenu principal : pleine largeur */
.main-content {
  grid-column: 1 / -1;
  margin: var(--space-06) 0;
}

/* Cartes : 4 colonnes chacune (desktop) / pleine largeur (mobile) */
.card {
  grid-column: span 4;
  background: var(--color-background-neutral);
  padding: var(--space-05);
  border-radius: 8px;
  border: 1px solid var(--color-background-strong);
}

/* === RESPONSIVE === */
@media (max-width: 768px) {
  .hero-section {
    grid-column: 1 / -1;
  }
  
  .sidebar {
    grid-column: 1 / -1;
  }
  
  .card {
    grid-column: 1 / -1;
  }
}
</style>