<template>
  <GridToggle @toggle="onGridToggle" />

  <SectionCover tag="Meet Milō" title="The all-in-one streaming&nbsp;amplifier"
    paragraph="One touch. Any source. Every room. Milō brings together Spotify, Bluetooth, and macOS streaming with powerful 60W amplification and seamless multiroom sync, all in one elegant system." />

  <SectionVideo desktop-video-src="/src/assets/videos/demo-desktop.mp4"
    mobile-video-src="/src/assets/videos/demo-mobile.mp4" :autoplay="false" />

  <SectionAudioSources tag="Audio Sources" title-prefix="Play audio from" />

  <SectionSplit variant="default">
    <template #content>
      <SectionTitle tag="Multiroom" title="Music everywhere in your home"
        paragraph="Create a perfectly synchronized multiroom ecosystem in every room. Smart group management, individual zone volume control, and simultaneous streaming with no noticeable lag."
        align="left" size="section" />
    </template>
    <template #illustration>
      <MultiroomIllustration />
    </template>
  </SectionSplit>

  <SectionSplit :reversed="true">
    <template #content>
      <SectionTitle tag="Equalizer" title="Your passive speakers, perfectly balanced."
        paragraph="Fine-tune your audio with 10 frequency bands (31Hz to 16kHz). Intuitive touch interface and real-time modifications to adapt the sound to your acoustics and tastes."
        align="left" size="section" />
    </template>
    <template #illustration>
      <EqualizerIllustration />
    </template>
  </SectionSplit>

<SectionRemoteControl 
  tag="Remote control" 
  title="Milō in your Phone and Mac" 
  :blocks="[
    {
      image: '/src/assets/images/remote-ios.jpg',
      tag: 'iOS · Android',
      title: 'Full control of Milō\nin your pocket.'
    },
    {
      image: '/src/assets/images/remote-mac.jpg',
      tag: 'macOS',
      title: 'Milō in your menu bar,\nlike it has always been here.'
    }
  ]" 
/>
</template>

<script>
import GridToggle from './components/GridToggle.vue'
import SectionTitle from './components/SectionTitle.vue'
import SectionSplit from './components/SectionSplit.vue'
import SectionCover from './components/SectionCover.vue'
import SectionVideo from './components/SectionVideo.vue'
import SectionAudioSources from './components/SectionAudioSources.vue'
import SectionRemoteControl from './components/SectionRemoteControl.vue'
import MultiroomIllustration from './components/MultiroomIllustration.vue'
import EqualizerIllustration from './components/EqualizerIllustration.vue'

export default {
  name: 'App',
  components: {
    GridToggle,
    SectionTitle,
    SectionSplit,
    SectionCover,
    SectionVideo,
    SectionAudioSources,
    SectionRemoteControl,
    MultiroomIllustration,
    EqualizerIllustration
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