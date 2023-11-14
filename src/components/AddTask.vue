<template>
    <form class="add-task_form" v-show="isInputVisible" @submit.prevent="handleSubmit" ref="formNode">
        <textarea class="add-task_textarea" v-model="inputedValue" ref="taskInputNode" />
        <div class="add-task_form__buttons">
            <button class="add-task_btn-add" type="submit">Add</button>
            <button class="add-task_btn-remove" @click="closeInput" type="button">X</button>
        </div>
    </form>

    <button class="add-task_btn" v-show="!isInputVisible" @click="openInput" type="button">
        {{ BUTTON_TEXT }}
    </button>
</template>

<script setup lang="ts">
import { ref, nextTick, type Ref } from 'vue';
import { onClickOutside } from '@vueuse/core';

const BUTTON_TEXT = '+ Add Task';

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

<style scoped>
.add-task_form {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.add-task_form__buttons {
    display: flex;
    justify-content: space-between;
}

.add-task_textarea {
    height: 80px;
    resize: none;
}

.add-task_btn-add {
    padding: 6px 12px;
    width: 100px;
    background: var(--main-accent, #6e6af0);
    border-radius: 6px;
    outline: none;
    color: var(--main-background, #fff);
}

.add-task_btn-remove {
    color: var(--main-label-text, #959ba3);

    &:hover {
        border-radius: 6px;
        background-color: #959ba321;
    }
}

.add-task_btn {
    padding: 6px 12px;
}

.add-task_textarea,
.add-task_btn {
    border: 0.5px solid var(--main-label-text, #959ba3);
    border-radius: 6px;
    background: var(--main-background, #fff);
    color: var(--main-label-text, #959ba3);
}
</style>
