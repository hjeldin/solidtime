<script setup lang="ts">
import type { Task } from '@/packages/api/src';
import { computed, ref } from 'vue';
import { Badge } from '@/packages/ui/src';
import TaskEditModal from '@/Components/Common/Task/TaskEditModal.vue';

const props = defineProps<{
    task: Task;
}>();

const formatCreatedAt = computed(() => {
    const date = new Date(props.task.created_at);
    return new Intl.DateTimeFormat(navigator.language, {
        timeZone: Intl.DateTimeFormat().resolvedOptions().timeZone,
        dateStyle: 'short',
        timeStyle: 'short'
    }).format(date);
});

const showTaskEditModal = ref(false);
</script>

<template>
    <div class="bg-card-background-active shadow rounded px-3 py-3 border border-card-border" @click="showTaskEditModal = true">
        <div class="flex justify-between">
            <p class="text-text-primary font-semibold font-sans tracking-wide">{{task.name}}</p>

<!--            <img-->
<!--                class="w-6 h-6 rounded-full ml-3"-->
<!--                src="https://pickaface.net/gallery/avatar/unr_sample_161118_2054_ynlrg.png"-->
<!--                alt="Avatar"-->
<!--            >-->
        </div>
        <div class="flex mt-4 justify-between items-center">
            <span class="text-sm text-text-secondary">{{formatCreatedAt}}</span>
            <badge v-if="task.type" :color="badgeColor">{{task.type}}</badge>
        </div>
        <TaskEditModal
            v-model:show="showTaskEditModal"
            :task="task"></TaskEditModal>
    </div>
</template>

<style scoped>

</style>
