<template>
  <svg v-if="name === 'play'" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none" class="icon">
    <path fill="currentColor" d="M7.5 17.157V6.843c0-.672.775-1.073 1.354-.7l8.008 5.157a.825.825 0 0 1 0 1.4l-8.008 5.157c-.579.373-1.354-.028-1.354-.7Z"/>
  </svg>
  <svg v-else-if="name === 'pause'" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none" class="icon">
    <path fill="currentColor" d="M6.75 6.75A.75.75 0 0 1 7.5 6h2.25a.75.75 0 0 1 .75.75v10.5a.75.75 0 0 1-.75.75H7.5a.75.75 0 0 1-.75-.75V6.75ZM13.5 6.75a.75.75 0 0 1 .75-.75h2.25a.75.75 0 0 1 .75.75v10.5a.75.75 0 0 1-.75.75h-2.25a.75.75 0 0 1-.75-.75V6.75Z"/>
  </svg>
  <svg v-else-if="name === 'previous'" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none" class="icon">
    <path fill="currentColor" d="M18.75 8.132v7.736c0 .504-.596.804-1.041.525l-6.16-3.868a.611.611 0 0 1 0-1.05l6.16-3.868c.445-.28 1.041.021 1.041.525Z"/>
    <path fill="currentColor" d="M11.25 8.132v7.736c0 .504-.596.804-1.041.525l-6.16-3.868a.611.611 0 0 1 0-1.05l6.16-3.868c.445-.28 1.041.021 1.041.525Z"/>
  </svg>
  <svg v-else-if="name === 'next'" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none" class="icon">
    <path fill="currentColor" d="M5.25 15.868V8.132c0-.504.596-.804 1.041-.525l6.16 3.868c.398.25.398.8 0 1.05l-6.16 3.868c-.445.28-1.041-.021-1.041-.525Z"/>
    <path fill="currentColor" d="M12.75 15.868V8.132c0-.504.596-.804 1.041-.525l6.16 3.868c.398.25.398.8 0 1.05l-6.16 3.868c-.445.28-1.041-.021-1.041-.525Z"/>
  </svg>
</template>

<script>
export default {
  name: 'Icon',
  props: {
    name: {
      type: String,
      required: true,
      validator(value) {
        return ['play', 'pause', 'previous', 'next'].includes(value)
      }
    },
    size: {
      type: [Number, String],
      default: 24
    }
  }
}
</script>

<style scoped>
.icon {
  display: block;
  pointer-events: none;
}
</style>
