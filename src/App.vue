<template>
  <GridToggle @toggle="onGridToggle" />

  <!-- Section principale avec SectionTitle main intégré -->
  <SectionCover 
    tag="Meet Milō" 
    title="The all-in-one streaming&nbsp;amplifier"
    paragraph="One touch. Any source. Every room. Milō brings together Spotify, Bluetooth, and macOS streaming with powerful 60W amplification and seamless multiroom sync, all in one elegant system." />

  <!-- SectionTitle classique simplifié -->
  <SectionTitle 
    tag="Fonctionnalités" 
    title="Architecture modulaire pour Raspberry Pi"
    paragraph="Un système pensé pour la flexibilité et la performance, avec une interface utilisateur moderne et responsive." />

  <!-- Section split normale -->
  <SectionSplit variant="default">
    <template #content>
      <SectionTitle 
        tag="Features" 
        title="Streaming multiplateforme" 
        paragraph="Compatible avec Spotify Connect, AirPlay et Bluetooth. Contrôlez votre musique depuis n'importe quelle source." />
    </template>
    <template #illustration>
      <!-- Animation future ici -->
    </template>
  </SectionSplit>

  <!-- Section split inversée avec fond contrast -->
  <SectionSplit variant="contrast" :reversed="true">
    <template #content>
      <SectionTitle 
        tag="Hardware" 
        title="Amplification 60W" 
        paragraph="Puce audio haute qualité avec amplification intégrée pour alimenter vos enceintes préférées." />
    </template>
    <template #illustration>
      <!-- Animation future ici -->
    </template>
  </SectionSplit>

</template>

<script>
import GridToggle from './components/GridToggle.vue'
import SectionTitle from './components/SectionTitle.vue'
import SectionSplit from './components/SectionSplit.vue'
import SectionCover from './components/SectionCover.vue'

export default {
  name: 'App',
  components: {
    GridToggle,
    SectionTitle,
    SectionSplit,
    SectionCover
  },
  methods: {
    onGridToggle(isVisible) {
      console.log('Grid visible:', isVisible)
    }
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  background-color: var(--color-background);
  color: var(--color-text);
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  gap: var(--grid-gutter);
  padding: 0 var(--grid-margin);
  max-width: 100%;
}
</style>