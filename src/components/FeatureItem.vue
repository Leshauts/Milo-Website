<!-- src / components / FeatureItem.vue -->
<template>
    <div class="feature-item">
        <div class="feature-item__icon">
            <!-- Icon SVG ou placeholder -->
            <img v-if="icon" :src="icon" :alt="name + ' icon'" class="svg-icon" />
            <div v-else class="icon-placeholder"></div>
        </div>
        <div class="feature-item__name body-medium">
            {{ name }}
        </div>
    </div>
</template>

<script>
export default {
    name: 'FeatureItem',
    props: {
        name: {
            type: String,
            required: true
        },
        icon: {
            type: String,
            default: null
        }
    }
}
</script>

<style scoped>
.feature-item {
    position: relative;
    background-color: var(--color-background-neutral-32);
    border-radius: var(--border-radius-medium);
    padding: var(--space-03) var(--space-03);
    display: flex;
    align-items: center;
    gap: var(--space-02);
    z-index: 0;
    overflow: hidden;
}

.feature-item::before {
    content: '';
    position: absolute;
    inset: 0;
    padding: 1.5px;
    background: var(--stroke-glass);
    border-radius: var(--border-radius-medium);
    -webkit-mask:
        linear-gradient(#000 0 0) content-box,
        linear-gradient(#000 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
    z-index: -1;
    pointer-events: none;
}

.feature-item__icon {
    width: 24px;
    height: 24px;
    flex-shrink: 0;
}

.svg-icon {
    width: 100%;
    height: 100%;
    object-fit: contain;
}

.icon-placeholder {
    width: 100%;
    height: 100%;
    background-color: var(--color-brand);
    border-radius: var(--border-radius-small);
}

.feature-item__name {
    color: var(--color-text);
    font-weight: 500;
}

/* === RESPONSIVE MOBILE === */
@media (max-width: 1024px) {
    .feature-item__icon {
        width: 20px;
        height: 20px;
    }
}
</style>