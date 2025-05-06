<script setup lang="ts">
import { useQuery } from "@tanstack/vue-query";
import { computed } from "vue";
import DashboardCard from "@/Components/Dashboard/DashboardCard.vue";
import { CheckCircleIcon } from "@heroicons/vue/20/solid";
import SecondaryButton from "@/packages/ui/src/Buttons/SecondaryButton.vue";
import { PlusCircleIcon } from "@heroicons/vue/24/solid";
import { router } from "@inertiajs/vue3";
import { getCurrentOrganizationId } from "@/utils/useUser";
import { api } from "@/packages/api/src";
import { LoadingSpinner } from "@/packages/ui/src";
import type { Project } from '@/types/models';
import RecentProjectCardEntry from '@/Components/Dashboard/RecentProjectCardEntry.vue';

// Get the organization ID using the utility function
const organizationId = computed(() => getCurrentOrganizationId());

// Function to fetch latest tasks using the API client

// Set up the query
const { data: projectsResponse, isLoading, refetch } = useQuery({
    queryKey: ["projects", organizationId],
    queryFn: () => {
        return api.getProjects({
            params: {
                organization: organizationId.value!
            },
            queries: {
            }
        });
    },
    enabled: computed(() => !!organizationId.value)
});

const latestProjects = computed(() => {
    if (!projectsResponse.value) {
        return [];
    }

    return projectsResponse.value.data;
});

const filteredLatestProjects = computed(() => {
    // do not include running time entries
    const finishedTimeEntries = latestProjects.value.filter((item) => item.end !== null);

    // filter out duplicates based on description, task, project, tags and billable
    return finishedTimeEntries.slice(0, 4);
});


// Listen for dashboard refresh events
window.addEventListener("dashboard:refresh", () => {
    refetch();
});
</script>

<template>
    <DashboardCard title="Recent Projects" :icon="CheckCircleIcon">
        <div v-if="isLoading" class="flex justify-center items-center h-40">
            <LoadingSpinner />
        </div>
        <div v-else-if="filteredLatestProjects && filteredLatestProjects.length > 0">
            <RecentProjectCardEntry v-for="project of filteredLatestProjects"
                             :project="project"
                             :key="project.id"></RecentProjectCardEntry>
        </div>
        <div
            v-else
            class="text-center flex flex-1 justify-center items-center">
            <div>
                <PlusCircleIcon
                    class="w-8 text-icon-default inline pb-2"></PlusCircleIcon>
                <h3 class="text-text-primary font-semibold text-sm">
                    No recent tasks found
                </h3>
                <p class="pb-5 text-sm">Create tasks inside of a project!</p>
                <SecondaryButton @click="router.visit(route('projects'))"
                >Go to Projects
                </SecondaryButton>
            </div>
        </div>
        <div
            v-if="latestProjects && latestProjects.length === 1"
            class="text-center flex flex-1 justify-center items-center text-sm">
            <div>
                <PlusCircleIcon
                    class="w-8 text-icon-default inline pb-2"></PlusCircleIcon>
                <h3 class="text-text-primary font-semibold">Add more tasks</h3>
                <p class="pb-5">Create tasks inside of a project!</p>
                <SecondaryButton @click="router.visit(route('projects'))"
                >Go to Projects
                </SecondaryButton>
            </div>
        </div>
    </DashboardCard>
</template>
