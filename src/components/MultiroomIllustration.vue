<template>
  <div class="multiroom-illustration">
    <!-- Background image -->
    <img src="/src/assets/images/multiroom/background.png" alt="" class="multiroom-illustration__background" />

    <!-- House image with color dodge blend mode -->
    <img src="/src/assets/images/multiroom/house.png" alt="" class="multiroom-illustration__house" />

    <!-- Title bar -->
    <div class="multiroom-illustration__title-bar">
      <div class="title-bar__left">
        <img src="/src/assets/images/multiroom/icon.png" alt="" class="title-bar__icon" />
        <h5 class="h5 title-bar__title">Multiroom</h5>
      </div>
      <div class="title-bar__toggle">
        <div class="toggle" :class="{ 'toggle--active': isActive }">
          <div class="toggle__knob"></div>
        </div>
      </div>
    </div>

    <!-- Room cards with associated speakers -->
    <div class="room-group room-group--kitchen">
      <div class="room-card room-card--kitchen">
        <span class="room-card__label body-small">Kitchen</span>
        <div class="room-card__toggle toggle-small" :class="{ 'toggle-small--active': rooms.kitchen }">
          <div class="toggle-small__knob"></div>
        </div>
      </div>
      <div class="speaker-icon speaker-icon--kitchen" :class="{ 'speaker-icon--active': rooms.kitchen }">
        <img src="/src/assets/images/multiroom/speakers.png" alt="" />
      </div>
    </div>

    <div class="room-group room-group--office">
      <div class="room-card room-card--office">
        <span class="room-card__label body-small">Office</span>
        <div class="room-card__toggle toggle-small" :class="{ 'toggle-small--active': rooms.office }">
          <div class="toggle-small__knob"></div>
        </div>
      </div>
      <div class="speaker-icon speaker-icon--office" :class="{ 'speaker-icon--active': rooms.office }">
        <img src="/src/assets/images/multiroom/speakers.png" alt="" />
      </div>
    </div>

    <div class="room-group room-group--living">
      <div class="room-card room-card--living">
        <span class="room-card__label body-small">Living room</span>
        <div class="room-card__toggle toggle-small" :class="{ 'toggle-small--active': rooms.living }">
          <div class="toggle-small__knob"></div>
        </div>
      </div>
      <div class="speaker-icon speaker-icon--living" :class="{ 'speaker-icon--active': rooms.living }">
        <img src="/src/assets/images/multiroom/speakers.png" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MultiroomIllustration',
  data() {
    return {
      isActive: true, // Main toggle state
      rooms: {
        kitchen: true,
        office: true,
        living: false
      }
    }
  }
}
</script>

<style scoped>
.multiroom-illustration {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: var(--border-radius-xxlarge);
}

/* === BACKGROUND === */
.multiroom-illustration__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* === HOUSE IMAGE === */
.multiroom-illustration__house {
  position: absolute;
  left: var(--space-07);
  right: var(--space-07);
  bottom: var(--space-09);
  width: calc(100% - var(--space-07) * 2);
  height: auto;
  mix-blend-mode: color-dodge;
}

/* === TITLE BAR === */
.multiroom-illustration__title-bar {
  position: absolute;
  top: 16%;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-03) var(--space-04) var(--space-03) var(--space-03);
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-large);
  gap: var(--space-05);
  width: calc(100% - (var(--space-08) * 2));
}

.title-bar__left {
  display: flex;
  align-items: center;
  gap: var(--space-03);
}

.title-bar__icon {
  width: 40px;
  height: 40px;
  border-radius: var(--border-radius-small);
}

.title-bar__title {
  margin: 0;
  color: var(--color-text);
}

/* === TOGGLE === */
.toggle {
  width: 56px;
  height: 32px;
  background-color: var(--color-background-strong);
  border-radius: var(--border-radius-full);
  position: relative;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle--active {
  background-color: var(--color-brand);
}

.toggle__knob {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 28px;
  height: 28px;
  background-color: var(--color-background-neutral);
  border-radius: var(--border-radius-full);
  transition: transform 0.3s ease;
}

.toggle--active .toggle__knob {
  transform: translateX(24px);
}

/* === ROOM GROUPS === */
.room-group {
  position: absolute;
  display: flex;
  align-items: center;
  z-index: 10;
}

/* === ROOM CARDS === */
.room-card {
  position: relative;
  display: flex;
  align-items: center;
  gap: var(--space-02);
  padding: var(--space-02) var(--space-03);
  backdrop-filter: blur(24px);
  background-color: var(--color-background-neutral-32);
  border-radius: var(--border-radius-full);
}

.room-card__label {
  margin: 0;
  color: var(--color-text);
  white-space: nowrap;
}

/* Small toggle for room cards */
.toggle-small {
  width: 28px;
  height: 16px;
  background-color: #00000014;
  border-radius: 8px;
  position: relative;
  cursor: pointer;
  transition: background-color 0.3s ease;
  flex-shrink: 0;
}

.toggle-small--active {
  background-color: var(--color-brand);
}

.toggle-small__knob {
  position: absolute;
  top: 1.5px;
  left: 1.5px;
  width: 13px;
  height: 13px;
  background-color: var(--color-background-neutral);
  border-radius: 50%;
  transition: transform 0.3s ease;
}

.toggle-small--active .toggle-small__knob {
  transform: translateX(12px);
}




/* Room group positions */
.room-group--kitchen {
  bottom: 47%;
  left: 13.5%;
  flex-direction: column;
  gap: var(--space-01);
}

.room-group--office {
  bottom: 47%;
  right: 14%;
  flex-direction: column;
  gap: var(--space-01);
}

.room-group--living {
  bottom: 32%;
  left: 50%;
  transform: translateX(-50%);
  flex-direction: column;
  gap: var(--space-01);
}

/* === SPEAKER ICONS === */
.speaker-icon {
  position: relative;
  width: 48px;
  height: 48px;
  padding: var(--space-02);
  border-radius: var(--border-radius-medium);
  backdrop-filter: blur(24px);
  background: var(--color-background-neutral-32);
}

.speaker-icon img {
  width: 100%;
  height: 100%;
  opacity: 0.32;
  transition: opacity 0.3s ease;
}

.speaker-icon--active img {
  opacity: 1;
}

/* === RESPONSIVE TABLET === */
@media (max-width: 1024px) {
  .multiroom-illustration__house {
    left: var(--space-06);
    right: var(--space-06);
    width: calc(100% - var(--space-06) * 2);
  }

  .multiroom-illustration__title-bar {
    width: calc(100% - (var(--space-07) * 2));
  }

  .speaker-icon {
    width: 40px;
    height: 40px;
  }
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 600px) {
  .multiroom-illustration__house {
    left: var(--space-07);
    right: var(--space-07);
    bottom: var(--space-09);
    width: calc(100% - var(--space-07) * 2);
  }

  .multiroom-illustration__title-bar {
    padding: var(--space-02) var(--space-03);
    gap: var(--space-04);
  }

  .title-bar__icon {
    width: 32px;
    height: 32px;
  }

  .toggle {
    width: 40px;
    height: 24px;
    border-radius: 12px;
  }

  .toggle__knob {
    width: 20px;
    height: 20px;
  }

  .toggle--active .toggle__knob {
    transform: translateX(16px);
  }

  .room-card {
    padding: var(--space-01) var(--space-02);
    gap: var(--space-01);
  }

  .toggle-small {
    width: 20px;
    height: 12px;
  }

  .toggle-small__knob {
    width: 10px;
    height: 10px;
    top: 1px;
    left: 1px;
  }

  .toggle-small--active .toggle-small__knob {
    transform: translateX(8px);
  }



  .speaker-icon {
    width: 32px;
    height: 32px;
  }
}
</style>
