<!-- src / components / SectionRemoteControl.vue -->
<template>
    <section class="section-remote-control">
        <SectionTitle :tag="tag" :title="title" :paragraph="paragraph" :align="align" :variant="variant" :size="size" />

        <div class="section-remote-control__blocks">
            <div v-for="(block, index) in blocks" :key="index" class="section-remote-control__block">
                <div class="section-remote-control__image-wrapper">
                    <img :src="block.image" :alt="block.title" class="section-remote-control__image" />
                </div>
                <div class="section-remote-control__tag body-small">
                    {{ block.tag }}
                </div>
                <h4 class="section-remote-control__block-title h4">
                    {{ block.title }}
                </h4>
            </div>
        </div>
    </section>
</template>

<script>
import SectionTitle from './SectionTitle.vue'

export default {
    name: 'SectionRemoteControl',
    components: {
        SectionTitle
    },
    props: {
        // Props pour SectionTitle
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
            default: null
        },
        align: {
            type: String,
            default: 'center'
        },
        variant: {
            type: String,
            default: 'default'
        },
        size: {
            type: String,
            default: 'section'
        },
        // Props pour les blocs
        blocks: {
            type: Array,
            required: true,
            validator: (blocks) => {
                return blocks.length === 2 && blocks.every(block =>
                    block.hasOwnProperty('image') &&
                    block.hasOwnProperty('tag') &&
                    block.hasOwnProperty('title')
                )
            }
        }
    }
}
</script>

<style scoped>
.section-remote-control {
    grid-column: 1 / -1;
    display: grid;
    grid-template-columns: subgrid;
    gap: var(--grid-gutter);
    row-gap: var(--space-08);
    background-color: var(--color-background-neutral);
    border-radius: var(--border-radius-large);
    padding: var(--space-09);
    overflow: hidden;
    margin-bottom: var(--space-09);
}

.section-remote-control__blocks {
    grid-column: 2 / -2;
    display: grid;
    grid-template-columns: subgrid;
    gap: var(--grid-gutter);
}

.section-remote-control__block {
    text-align: center;
}

.section-remote-control__block:first-child {
    grid-column: 1 / 4;
}

.section-remote-control__block:last-child {
    grid-column: 4 / 7;
}

.section-remote-control__image-wrapper {
    width: 100%;
    aspect-ratio: 1 / 1;
    border-radius: var(--border-radius-large);
    overflow: hidden;
}

.section-remote-control__image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.section-remote-control__tag {
    color: var(--color-brand);
    margin-top: var(--space-05);
}

.section-remote-control__block-title {
    color: var(--color-text);
    margin-top: var(--space-02);
    white-space: pre-line;
}

/* === RESPONSIVE TABLET === */
@media (max-width: 1024px) {
    .section-remote-control {
        padding: var(--space-09) var(--space-07);
    }

    .section-remote-control__blocks {
        grid-column: 2 / -2;
    }
        .section-remote-control__block-title {
        white-space:normal;
        text-wrap: balance;
    }

}

/* === RESPONSIVE MOBILE === */
@media (max-width: 600px) {
    .section-remote-control {
        padding: var(--space-09) var(--space-08);
    }

    .section-remote-control__blocks {
        grid-column: 1 / -1;
        /* grid-template-columns: 1fr; */
        row-gap: var(--space-08);
    }

    .section-remote-control__block:first-child,
    .section-remote-control__block:last-child {
        grid-column: 1 / 5;
    }
        .section-remote-control__block-title {
        white-space:pre-line;
    }
}
</style>