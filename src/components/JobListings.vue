<script setup>
import { RouterLink } from "vue-router";
import JobListing from "./JobListing.vue"
import { onMounted, ref, reactive } from "vue";
import axios from "axios";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    }
})

const reactiveState = reactive({
    jobs:[],
    isLoading: true
});

onMounted(async ()=>{
    try {
        const response = await axios.get('/api/jobs');
        reactiveState.jobs = response.data;
    } catch (error) {
        console.error("Error Fetching Jobs", error);
    } finally {
        reactiveState.isLoading = false;
    }
});
</script>

<template>
    <section class="bg-green-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>
            <!-- Show Loading Spinner while Loading is True -->
            <div v-if="reactiveState.isLoading" class="text-center text-gray-500 py-6">
                <PulseLoader />
            </div>
            <!-- Show Jobs Listing when Loading is False -->
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListing v-for="job in reactiveState.jobs.slice(0, limit || reactiveState.jobs.length)" :key="job.id" :job="job" />
            </div>
        </div>
    </section>
    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink to="/jobs" class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700">
            View All Jobs
        </RouterLink>
    </section>
</template>