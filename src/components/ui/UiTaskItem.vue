<template>
    <div class="task_item">
        {{ props.task.title }}
        <UiDragIcon class="task_item__drag-icon" />
    </div>
</template>

<script setup lang="ts">
import type { Task } from '@/types';
import UiDragIcon from './UiDragIcon.vue';

const props = defineProps<{
    task: Task;
}>();
</script>

<style scoped>
.task_item {
    display: flex;

    position: relative;
    padding: 12px;
    border-radius: 6px;
    background: var(--main-background, #fff);
}

.task_item__drag-icon {
    position: absolute;
    right: 6px;
}
</style>
