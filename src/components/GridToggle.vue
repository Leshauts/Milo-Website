<template>
    <div>
        <!-- Toggle de grille -->
        <div class="grid-toggle" :class="{ active: isVisible }" @click="toggle"
            :title="isVisible ? 'Masquer la grille (G)' : 'Afficher la grille (G)'"></div>

        <!-- Overlay de grille -->
        <Teleport to="body">
            <div class="grid-overlay" :class="{ visible: isVisible }" v-if="isVisible">
                <div class="grid-overlay-inner">
                    <div v-for="n in columnCount" :key="n" class="grid-overlay-column"></div>
                </div>
            </div>
        </Teleport>
    </div>
</template>

<script>
export default {
    name: 'GridToggle',
    data() {
        return {
            isVisible: false,
            windowWidth: window.innerWidth
        }
    },
    computed: {
        isMobile() {
            return this.windowWidth <= 600
        },
        columnCount() {
            return this.isMobile ? 4 : 8
        }
    },
    mounted() {
        window.addEventListener('resize', this.handleResize)
        document.addEventListener('keydown', this.handleKeydown)
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.handleResize)
        document.removeEventListener('keydown', this.handleKeydown)
    },
    methods: {
        toggle() {
            this.isVisible = !this.isVisible
            this.$emit('toggle', this.isVisible)
        },
        handleResize() {
            this.windowWidth = window.innerWidth
        },
        handleKeydown(e) {
            if (e.key.toLowerCase() === 'g' && !e.ctrlKey && !e.metaKey && !e.altKey) {
                if (!['INPUT', 'TEXTAREA'].includes(e.target.tagName)) {
                    e.preventDefault()
                    this.toggle()
                }
            }
        }
    },
    emits: ['toggle']
}
</script>

<style scoped>
/* === TOGGLE DE GRILLE === */
.grid-toggle {
    position: fixed;
    bottom: 16px;
    left: 16px;
    width: 32px;
    height: 32px;
    background: var(--color-background-contrast);
    border: 1px solid var(--color-text-light);
    border-radius: 4px;
    cursor: pointer;
    z-index: 9999;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    opacity: 0.3;
}

.grid-toggle:hover {
    opacity: 0.8;
    transform: scale(1.05);
}

.grid-toggle.active {
    background: var(--color-brand);
    border-color: var(--color-brand);
    opacity: 1;
}

.grid-toggle::before {
    content: '';
    width: 12px;
    height: 12px;
    background-image:
        linear-gradient(to right, var(--color-text-contrast) 2px, transparent 2px),
        linear-gradient(to bottom, var(--color-text-contrast) 2px, transparent 2px);
    background-size: 4px 4px;
    background-repeat: repeat;
}

@media (max-width: 1024px) {
    .grid-toggle {
        bottom: 12px;
        left: 12px;
        width: 28px;
        height: 28px;
    }

    .grid-toggle::before {
        width: 10px;
        height: 10px;
        background-size: 3px 3px;
    }
}
</style>

<style>
/* === OVERLAY DE GRILLE (global styles) === */
.grid-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    pointer-events: none;
    z-index: 9998;
    display: none;
}

.grid-overlay.visible {
    display: block;
}

.grid-overlay-inner {
    display: grid;
    grid-template-columns: repeat(var(--grid-columns), 1fr);
    gap: var(--grid-gutter);
    padding: 0 var(--grid-margin);
    height: 100vh;
    max-width: 100%;
    margin: 0 auto;
}

.grid-overlay-column {
    background: rgba(239, 100, 46, 0.1);
    border: 1px solid rgba(239, 100, 46, 0.2);
}

/* Pas besoin de règle spécifique pour tablet car les variables CSS s'adaptent automatiquement */

/* Seulement si vous avez besoin d'ajustements spécifiques au mobile */
@media (max-width: 600px) {
    .grid-overlay-inner {
        /* Les variables --grid-columns, --grid-margin et --grid-gutter 
           se mettent automatiquement à jour grâce au design-system.css */
    }
}
</style>