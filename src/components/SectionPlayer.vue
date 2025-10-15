<template>
  <section class="section-player" :class="`section-player--${variant}`">
    <!-- Animated Gradients -->
    <div class="animated-gradients">
      <div class="gradient gradient-0" :class="{ 'gradient-active': activeSourceIndex === 0 }"></div>
      <div class="gradient gradient-1" :class="{ 'gradient-active': activeSourceIndex === 1 }"></div>
      <div class="gradient gradient-2" :class="{ 'gradient-active': activeSourceIndex === 2 }"></div>
    </div>

    <!-- Fake Milo Player -->
    <div class="section-player__app">
      <div class="player-container">
        <!-- Player Interface -->
        <div class="player-interface">
          <!-- Album Art Section -->
          <div class="album-art-section">
            <div class="album-art-container">
              <!-- Blur background -->
              <div class="album-art-blur" :style="{ backgroundImage: `url(${albumCover})` }"></div>

              <!-- Main album art -->
              <div class="album-art">
                <img :src="albumCover" alt="Album Art" />
              </div>
            </div>
          </div>

          <!-- Content Section -->
          <div class="content-section">
            <!-- Track Info -->
            <div class="track-info">
              <h1 class="track-title h4">{{ trackTitle }}</h1>
              <p class="track-artist h5">{{ trackArtist }}</p>
            </div>

            <!-- Controls Section -->
            <div class="controls-section">
              <!-- Progress Bar -->
              <div class="progress-wrapper">
                <div class="progress-bar">
                  <span class="body-mono time">{{ currentTime }}</span>
                  <div class="progress-container">
                    <div class="progress" :style="{ width: progressWidth }"></div>
                  </div>
                  <span class="body-mono time">{{ duration }}</span>
                </div>
              </div>

              <!-- Playback Controls -->
              <div class="controls-wrapper">
                <div class="controls">
                  <div class="control-button previous">
                    <Icon name="previous" :size="48" class="icon-secondary" />
                  </div>
                  <div class="control-button play-pause">
                    <Icon name="play" :size="48" class="icon-primary" />
                  </div>
                  <div class="control-button next">
                    <Icon name="next" :size="48" class="icon-secondary" />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Bottom Navigation (3 icons) - Positioned absolutely over player -->
        <div class="dock" :class="{ 'dock-visible': isDockVisible }" @click="handleDockClick">
          <button v-for="(source, index) in audioSources" :key="source.id" class="dock-item"
            :class="{ 'dock-item-visible': isDockVisible }"
            :style="{ transitionDelay: isDockVisible ? `${0.05 + index * 0.08}s` : '0s' }">
            <AppIcon :name="source.icon" size="large" class="dock-item-icon" />
          </button>
          <div class="active-indicator" :style="indicatorStyle"></div>
        </div>
      </div>
    </div>

    <!-- Text Content -->
    <div class="section-player__content">
      <SectionTitle :tag="tag" :title="title" :paragraph="paragraph" align="left" size="section" :variant="variant" />
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import SectionTitle from './SectionTitle.vue';
import AppIcon from './AppIcon.vue';
import Icon from './Icon.vue';

const props = defineProps({
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
    required: true
  },
  variant: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'contrast'].includes(value)
  }
});

// Static player data
const trackTitle = 'Angel Dust';
const trackArtist = 'Mac Miller';
const duration = '3:42';
const currentTime = '1:24';
const progressWidth = '37.8%'; // (1:24 / 3:42) * 100
const albumCover = '/src/assets/images/angel-dust-cover.jpg';

// Audio sources
const audioSources = [
  { id: 'spotify', icon: 'librespot' },
  { id: 'bluetooth', icon: 'bluetooth' },
  { id: 'macos', icon: 'roc' }
];

const activeSourceIndex = ref(0);
const isDockVisible = ref(false);
let animationTimeout = null;

// Indicator style with delay
const indicatorStyle = computed(() => {
  const itemWidth = 56; // icon size
  const gap = 8; // var(--space-02)
  const offsetX = activeSourceIndex.value * (itemWidth + gap) + (itemWidth / 2) - 2;

  return {
    opacity: isDockVisible.value ? '1' : '0',
    transform: `translateX(${offsetX}px)`,
    transition: isDockVisible.value
      ? 'opacity 0.3s ease 0.4s, transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)'
      : 'opacity 0.15s ease, transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)'
  };
});

// Handle dock click animation
const handleDockClick = () => {
  if (animationTimeout) return; // Prevent multiple clicks

  // Hide dock
  isDockVisible.value = false;

  // Show dock after 2 seconds with stagger
  animationTimeout = setTimeout(() => {
    isDockVisible.value = true;

    // Clear timeout reference after animation completes
    setTimeout(() => {
      animationTimeout = null;
    }, 1000);
  }, 2000);
};

onMounted(() => {
  // Show dock on mount with stagger animation
  setTimeout(() => {
    isDockVisible.value = true;
  }, 300);
});
</script>

<style scoped>
/* === SECTION LAYOUT === */
.section-player {
  position: relative;
  display: grid;
  grid-template-columns: subgrid;
  grid-column: 1 / -1;
  border-radius: var(--border-radius-xxlarge);
  background-color: var(--color-background-neutral);
  align-items: center;
  overflow: hidden;
}

/* === PLAYER LAYOUT === */
.section-player__app {
  grid-column: 1 / 5;
  display: flex;
  flex-direction: column;
  padding: var(--space-09) 0 var(--space-09) var(--space-09);
  position: relative;
  z-index: 2;
}

.section-player__content {
  grid-column: 5 / -1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 var(--space-08) 0 calc(var(--space-08) + 16px);
  position: relative;
  z-index: 2;
}

.player-container {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: var(--space-04);
  background: var(--color-background-neutral);
  border-radius: var(--border-radius-xxlarge);
  overflow: hidden;
  aspect-ratio: 5 / 3;
}

/* === PLAYER INTERFACE === */
.player-interface {
  flex: 1;
  display: flex;
  padding: var(--space-04);
  gap: var(--space-05);
  min-height: 0;
}

/* Album Art */
.album-art-section {
  flex-shrink: 0;
  flex-grow: 0;
  aspect-ratio: 1;
  height: 100%;
}

.album-art-container {
  position: relative;
  width: 100%;
  height: 100%;
}

.album-art-blur {
  position: absolute;
  top: -20px;
  left: -20px;
  right: -20px;
  bottom: -20px;
  z-index: 2;
  background-size: cover;
  background-position: center;
  filter: blur(var(--space-07)) saturate(1.5);
  transform: scale(1.1) translateZ(0);
  opacity: .25;
  will-change: transform, filter;
  backface-visibility: hidden;
}

.album-art {
  position: relative;
  z-index: 3;
  width: 100%;
  height: 100%;
  border-radius: var(--border-radius-large);
  overflow: hidden;
  box-shadow: 0px 0px 64px 0px #0000000d;
  pointer-events: none;
}

.album-art img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Content Section */
.content-section {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.track-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  gap: var(--space-02);
  padding-top: var(--space-04);
}

.track-title {
  color: var(--color-text);
}

.track-artist {
  color: var(--color-text-light);
}

.controls-section {
  display: flex;
  flex-direction: column;
  gap: var(--space-04);
}

/* Progress Bar */
.progress-bar {
  display: flex;
  align-items: center;
  width: 100%;
  gap: var(--space-03);
}

.progress-container {
  flex-grow: 1;
  height: 8px;
  background-color: var(--color-background-strong);
  border-radius: 4px;
  position: relative;
  overflow: hidden;
}

.progress {
  height: 100%;
  background-color: var(--color-background-contrast);
  border-radius: 4px;
  position: absolute;
  left: 0;
}

.time {
  color: var(--color-text-light);
}

/* Playback Controls */
.controls {
  background: var(--color-background);
  border-radius: var(--border-radius-large);
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding: var(--space-01) var(--space-03);
}

.control-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 64px;
  height: 64px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.control-button.play-pause {
  width: 72px;
  height: 72px;
}

.icon-primary {
  color: var(--color-text);
  pointer-events: none;
}

.icon-secondary {
  color: var(--color-text-light);
  pointer-events: none;
}

/* === BOTTOM NAVIGATION (DOCK) === */
.dock {
  position: absolute;
  bottom: var(--space-04);
  left: 50%;
  transform: translateX(-50%) translateY(80px) scale(0.85);
  z-index: 10;

  border-radius: var(--border-radius-xxlarge);
  padding: var(--space-04);
  background: rgba(120, 120, 120, 0.16);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);

  display: flex;
  flex-direction: row;
  align-items: center;
  gap: var(--space-03);

  cursor: pointer;
  opacity: 0;
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.dock.dock-visible {
  opacity: 1;
  transform: translateX(-50%) translateY(0) scale(1);
}

.dock::before {
  content: '';
  position: absolute;
  inset: 0;
  padding: 1.5px;
  background: var(--stroke-glass);
  border-radius: var(--border-radius-xxlarge);
  -webkit-mask:
    linear-gradient(#000 0 0) content-box,
    linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  z-index: -1;
  pointer-events: none;
}

.dock-item {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
  opacity: 0;
  transform: translateY(20px) scale(0.8);
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.dock-item.dock-item-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.dock-item-icon {
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.active-indicator {
  position: absolute;
  bottom: 8px;
  left: 0;
  width: 6px;
  height: 4px;
  background: var(--color-background-contrast);
  border-radius: var(--border-radius-full);
  pointer-events: none;
  transition: all 0.7s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* === ANIMATED GRADIENTS === */
.animated-gradients {
  position: absolute;
  top: 50%;
  left: 20%;
  transform: translate(-50%, -50%);
  z-index: 0;
  pointer-events: none;
  width: 80vw;
  height: 80vw;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.gradient {
  position: absolute;
  width: 80vw;
  height: 80vw;
  top: 0;
  left: 0;
  filter: blur(100vw);
  animation: rotate 15s linear infinite;
  border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
  opacity: 0;
  transition: opacity 0.6s ease;
}

.gradient.gradient-active {
  opacity: 1;
}

.gradient-0 {
  background-image: linear-gradient(rgba(255, 187, 0, 0.4), rgba(231, 162, 0, 0.24));
}

.gradient-1 {
  background-image: linear-gradient(rgba(0, 76, 255, 0.24), rgba(34, 60, 104, 0.08));
}

.gradient-2 {
  background-image: linear-gradient(rgba(163, 173, 194, 0.24), rgba(205, 221, 228, 0.48));
}

/* === RESPONSIVE - TABLET === */
@media (max-width: 1024px) {
  .section-player__content {
    padding: 0 var(--space-06) 0 var(--space-06);
  }
}

/* === RESPONSIVE - MOBILE === */
@media (max-width: 820px) {
  .section-player__app {
    grid-column: 1 / -1;
    padding: var(--space-06);
  }

  .section-player__content {
    grid-column: 1 / -1;
    padding: var(--space-09) var(--space-06) var(--space-09) var(--space-06);
  }

  .player-interface {
    padding: var(--space-05) var(--space-05) 0 var(--space-05);
    flex-direction: column;
    gap: 0;
  }

  .album-art-section {
    max-width: 100%;
  }

  .album-art-blur {
    transform: scale(1) translateZ(0);
  }

  .track-info {
    padding: var(--space-06) 0 var(--space-03) 0;
  }

  .dock {
    bottom: var(--space-05);
  }

  .dock-item-icon {
    width: 64px;
    height: 64px;
  }

  .animated-gradients {
    left: 50%;
    width: 120vw;
    height: 120vw;
  }

  .gradient {
    width: 120vw;
    height: 120vw;
    filter: blur(calc(120vw / 5));
  }
}
</style>
