<template>
    <div class="p-4">
        <!-- Checkbox Filters -->
        <div class="mb-4">
            <label class="inline-flex items-center mr-4">
                <input type="checkbox" value="laravel" v-model="selectedTags" class="form-checkbox h-5 w-5 text-blue-600">
                <span class="ml-2 text-gray-700">Laravel</span>
            </label>
            <label class="inline-flex items-center mr-4">
                <input type="checkbox" value="vue" v-model="selectedTags" class="form-checkbox h-5 w-5 text-blue-600">
                <span class="ml-2 text-gray-700">Vue</span>
            </label>
            <label class="inline-flex items-center mr-4">
                <input type="checkbox" value="vue.js" v-model="selectedTags" class="form-checkbox h-5 w-5 text-blue-600">
                <span class="ml-2 text-gray-700">Vue.js</span>
            </label>
            <label class="inline-flex items-center mr-4">
                <input type="checkbox" value="php" v-model="selectedTags" class="form-checkbox h-5 w-5 text-blue-600">
                <span class="ml-2 text-gray-700">PHP</span>
            </label>
            <label class="inline-flex items-center mr-4">
                <input type="checkbox" value="api" v-model="selectedTags" class="form-checkbox h-5 w-5 text-blue-600">
                <span class="ml-2 text-gray-700">API</span>
            </label>
        </div>
        <!-- Filter Button -->
        <button @click="getLinks" class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-400">
            Filter
        </button>

        <!-- Links Table -->
        <div v-if="links.length > 0" class="mt-6">
            <table class="min-w-full bg-white border border-gray-200 rounded-lg shadow-md">
                <thead class="bg-gray-100">
                    <tr>
                        <th class="text-center py-2 px-4 border-b border-gray-300 text-left text-gray-700">URL</th>
                        <th class="text-center py-2 px-4 border-b border-gray-300 text-left text-gray-700">Title</th>
                        <th class="text-center py-2 px-4 border-b border-gray-300 text-left text-gray-700">Comments</th>
                        <th class="text-center py-2 px-4 border-b border-gray-300 text-left text-gray-700">Tags</th>
                        <th class="text-center py-2 px-4 border-b border-gray-300 text-left text-gray-700">Status</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="link in links" :key="link.id" class="hover:bg-gray-50">
                        <td class="py-2 px-4 border-b border-gray-200">
                            <a :href="link.url" target="_blank" class="text-blue-500 hover:underline">{{ link.url }}</a>
                        </td>
                        <td class="py-2 px-4 border-b border-gray-200">{{ link.title }}</td>
                        <td class="py-2 px-4 border-b border-gray-200">{{ link.comments }}</td>
                        <td class="py-2 px-4 border-b border-gray-200">{{ link.tags }}</td>
                        <td class="py-2 px-4 border-b border-gray-200">
                            <span v-if="!link.is_live" class="text-red-500">Link is dead</span>
                            <span v-else class="text-green-500">Live</span>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>


<script>
import axios from 'axios';

export default {
    data() {
        return {
            selectedTags: [],
            links: []
        };
    },
    methods: {
        // Retrieve links based on provided tags
        async getLinks() {
            try {
                const response = await axios.get('http://127.0.0.1:8000/api/get-links', {
                    params: {
                        tags: this.selectedTags
                    }
                });
                this.links = response.data;
            } catch (error) {
                console.error('There was an error getting links:', error);
            }
        }
    },
    mounted() {
        this.getLinks();
    }
};
</script>