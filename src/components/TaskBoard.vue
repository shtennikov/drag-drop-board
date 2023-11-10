<template>
    <div class="board">
        <UiColumn v-slot="{ tasks }" :columns="boardColumns">
            <UiTaskItem v-for="task in tasks" :key="task.id" :task="task" />
        </UiColumn>
    </div>
</template>

<script setup lang="ts">
// eslint-disable-next-line import/no-unresolved, import/extensions
import mockData from '@/mockData';
import type { Column } from '@/types';
import UiColumn from './ui/UiColumn.vue';
import UiTaskItem from './ui/UiTaskItem.vue';

const boardColumns: Column[] = mockData;
</script>

<style scoped>
.board {
    display: flex;
    align-items: flex-start;
    gap: 18px;
    padding: 6px 18px;
    overflow: auto;
    background-color: var(--secondary-background, #f7f7f7);
}

/* Scrollbar */
.board::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 1px rgba(0, 0, 0, 0.3);
}

.board::-webkit-scrollbar {
    height: 10px;
    background-color: #f5f5f5;
}

.board::-webkit-scrollbar-thumb {
    background-color: #959ba36e;
    border-radius: 6px;
}
</style>
