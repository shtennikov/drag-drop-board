<template>
    <div class="column">
        <div class="column__header">
            <span class="column__title">
                {{ column.title }}
            </span>
            <UiDragIcon class="column__drag-icon" />
        </div>

        <slot />
    </div>
</template>

<script setup lang="ts">
import type { Column } from '@/types';
import UiDragIcon from './UiDragIcon.vue';

defineProps<{
    column: Column;
}>();
</script>

<style scoped>
.column {
    display: flex;
    flex-direction: column;
    gap: 8px;
    min-width: var(--column-width);
}

.column__header {
    display: flex;
    position: relative;
    justify-content: center;
    align-items: center;
}

.column__drag-icon {
    position: absolute;
    right: 0;
}
</style>
