<template>
    <draggable v-model="boardColumns" group="columns" item-key="id" class="board" handle=".drag_icon" :animation="180">
        <template #item="{ element: column }: { element: Column }">
            <UiColumn :column="column">
                <draggable
                    v-model="column.tasks"
                    :group="{ name: 'tasks', pull: isAltPressed ? 'clone' : true }"
                    item-key="id"
                    handle=".drag_icon"
                    :animation="150"
                    :style="{ display: 'flex', gap: '8px', 'flex-direction': 'column' }"
                >
                    <template #item="{ element: task }: { element: Task }">
                        <UiTaskItem :task="task" />
                    </template>
                </draggable>

                <AddTask @add="addTask($event, column.tasks)" />
            </UiColumn>
        </template>
    </draggable>
</template>

<script setup lang="ts">
import draggable from 'vuedraggable';
import { nanoid } from 'nanoid';
import { ref, type Ref } from 'vue';
import { useKeyModifier } from '@vueuse/core';
// eslint-disable-next-line import/no-unresolved, import/extensions
import mockData from '@/mockData';
import type { Column, Task } from '@/types';
import UiColumn from './ui/UiColumn.vue';
import UiTaskItem from './ui/UiTaskItem.vue';
import AddTask from './AddTask.vue';

const boardColumns: Ref<Column[]> = ref(mockData);
const isAltPressed = useKeyModifier('Alt');

function addTask(value: string, tasks: Task[]): void {
    tasks.push({
        id: nanoid(),
        title: value,
        createdAt: new Date(),
    });
}
</script>

<style scoped>
.board {
    display: flex;
    align-items: flex-start;
    gap: 18px;
    padding: 6px 18px;
    overflow-x: auto;
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
    background-color: var(--main-label-text, #959ba3);
    border-radius: 6px;
}

/* drag and drop styles */
/* .sortable-chosen {

}

.sortable-drag {

} */

.sortable-ghost.task_item {
    background-color: var(--secondary-accent);
}
</style>
