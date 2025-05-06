<script setup lang="ts">
import ProjectBadge from '@/packages/ui/src/Project/ProjectBadge.vue';
import { useProjectsStore } from '@/utils/useProjects';
import { storeToRefs } from 'pinia';
import { computed } from 'vue';
import type { Project } from '@/types/models';
import { Link } from '@inertiajs/vue3';

const props = defineProps<{
    project: Project
}>();

const { projects } = storeToRefs(useProjectsStore());

const project = computed(() => {
    return props.project;
});

</script>

<template>
    <div
        class="px-3.5 py-2 grid grid-cols-5 border-b border-b-card-background-separator">
        <Link :href="route('projects.show', project.id)" class="col-span-1 flex items-center">
            <div class="col-span-4">
                <ProjectBadge
                    size="base"
                    class="min-w-0 max-w-full"
                    :color="project?.color">
                    <div class="flex items-center lg:space-x-0.5 min-w-0">
                        <span class="whitespace-nowrap ">
                            {{ project?.name ?? 'No Project' }}
                        </span>
                    </div>
                </ProjectBadge>
            </div>
        </Link>
    </div>
</template>
