<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { twMerge } from 'tailwind-merge';

const props = defineProps<{
    name?: string;
    class?: string;
}>();

const input = ref<HTMLInputElement | null>(null);

onMounted(() => {
    if (input.value?.hasAttribute('autofocus')) {
        input.value?.focus();
    }
});

defineExpose({ focus: () => input.value?.focus() });
const model = defineModel();
</script>

<template>
    <textarea
        ref="input"
        :class="
            twMerge(
                'border-input-border border bg-input-background text-text-primary focus-visible:ring-2 focus-visible:ring-ring focus-visible:border-transparent rounded-md shadow-sm',
                props.class
            )
        "
        v-model="model"
        :name="name">
    </textarea>
</template>
