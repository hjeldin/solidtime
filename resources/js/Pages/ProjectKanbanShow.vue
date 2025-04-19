<script setup lang="ts">
import draggable from "vuedraggable";
import TaskCard from '@/Components/Common/Task/TaskCard.vue';
import type { Task } from '@/packages/api/src';
import { SecondaryButton } from '@/packages/ui/src';
import { FolderIcon, PlusIcon } from '@heroicons/vue/16/solid';
import TaskCreateModal from '@/Components/Common/Task/TaskCreateModal.vue';
import {
    canCreateProjects,
    canCreateTasks,
    canViewProjectMembers,
} from '@/utils/permissions';
import { ref } from 'vue';

const props = defineProps<{
    projectId: string,
    columns: {
        name: string;
        tasks: Task[];
    }[];
}>();

const createTask = ref(false);
</script>

<template>
    <div class="flex">
        <div class="h-100 flex overflow-x-scroll pt-8 pb-12">
            <div
                v-for="column in columns"
                :key="column.name"
                class="bg-card-background rounded-lg px-3 py-3 column-width mr-4"
            >
                <p class="text-text-primary font-semibold font-sans tracking-wide text-sm">{{column.name}}</p>
                <draggable item-key="id" :list="column.tasks" :animation="200" ghost-class="ghost-card" group="tasks">
                    <template #item="{element}">
                        <TaskCard
                            :key="element.id"
                            :task="element"
                            class="mt-3 cursor-pointer"
                        ></TaskCard>
                    </template>
                </draggable>
                <div class="flex justify-center items-center mt-4">
                    <SecondaryButton
                        v-if="canCreateTasks()"
                        :icon="PlusIcon"
                        @click="createTask = true"
                        class="w-full h-16"
                        >Create Task
                    </SecondaryButton>
                    <TaskCreateModal
                        v-model:show="createTask"
                        :project-id="projectId"></TaskCreateModal>
                </div>
            </div>
            <div class="bg-card-background rounded-lg px-3 py-3 column-width mr-4 flex justify-center items-center">
                <SecondaryButton
                    v-if="canCreateTasks()"
                    :icon="PlusIcon"
                    @click="createTask = true"
                    class="w-full h-16"
                >Create Column
                </SecondaryButton>
            </div>
        </div>
    </div>
</template>

<style scoped>
.column-width {
    min-width: 320px;
    width: 320px;
}

.ghost-card {
    @apply border opacity-50 border-blue-500 bg-card-background
}
</style>
