<template>
  <GridToggle @toggle="onGridToggle" />
  
  <!-- Section principale avec MainSectionTitle -->
  <Section variant="default">
    <SectionTitle 
      variant="main"
      tag="Meet Milō"
      title="The all-in-one streaming&nbsp;amplifier"
      paragraph="One touch. Any source. Every room. Milō brings together Spotify, Bluetooth, and macOS streaming with powerful 60W amplification and seamless multiroom sync, all in one elegant system."
    />
  </Section>

  <!-- Section fonctionnalités -->
  <Section variant="strong">
    <SectionTitle 
      tag="Fonctionnalités"
      title="Architecture modulaire pour Raspberry Pi"
      paragraph="Un système pensé pour la flexibilité et la performance, avec une interface utilisateur moderne et responsive."
    />
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


</style>