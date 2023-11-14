<template>
    <form v-show="isInputVisible" @submit.prevent="handleSubmit" ref="formNode">
        <input v-model="inputedValue" ref="taskInputNode" />
        <button type="submit">Add</button>
        <button @click="closeInput" type="button">X</button>
    </form>

    <button v-show="!isInputVisible" @click="openInput" type="button">
        {{ BUTTON_TEXT }}
    </button>
</template>

<script setup lang="ts">
import { ref, nextTick, type Ref } from 'vue';
import { onClickOutside } from '@vueuse/core';

const BUTTON_TEXT = 'Add Task';

const emit = defineEmits<{
    add: [inputedValue: string];
}>();

const formNode: Ref<null | HTMLElement> = ref(null);
const taskInputNode: Ref<null | HTMLInputElement> = ref(null);
const isInputVisible = ref(false);
const inputedValue = ref('');

const openInput = (): void => {
    isInputVisible.value = true;

    nextTick().then(() => {
        taskInputNode.value?.focus();
    });
};

const closeInput = (): void => {
    inputedValue.value = '';
    isInputVisible.value = false;
};

const handleSubmit = (): void => {
    if (!inputedValue.value.length) {
        closeInput();
        return;
    }

    emit('add', inputedValue.value);
    closeInput();
};

onClickOutside(formNode, () => {
    if (!isInputVisible.value) return;

    if (inputedValue.value.length) {
        handleSubmit();
    } else {
        closeInput();
    }
});
</script>

<style scoped></style>
