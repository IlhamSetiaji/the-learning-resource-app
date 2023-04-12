<template>
    <base-card>
        <base-button @click="selectTab('stored-resource')" :mode="storedResourceSelected">Stored Resources</base-button>
        <base-button @click="selectTab('add-resource')" :mode="addResourceSelected">Add Resource</base-button>
    </base-card>

    <keep-alive>
        <component :is='selectedTab'></component>
    </keep-alive>
</template>

<script>
import StoredResource from './StoredResource.vue';
import AddResource from './AddResource.vue';

export default {
    components: {
        StoredResource,
        AddResource
    },
    computed: {
        storedResourceSelected(){
            return this.selectedTab === 'stored-resource' ? '' : 'flat';
        },
        addResourceSelected(){
            return this.selectedTab === 'add-resource' ? '' : 'flat';
        },
    },
    data() {
        return {
            storedResources:[
                {
                    id: 'official-guide',
                    title: 'Official Guide',
                    description: 'The official guide to Vue.js',
                    link: 'https://vuejs.org/'
                },
                {
                    id: 'google',
                    title: 'Google',
                    description: 'Google search engine',
                    link: 'https://www.google.com/'
                }
            ],
            selectedTab: 'stored-resource'
        }
    },
    provide() {
        return {
            resources: this.storedResources,
            addResource: this.addResource,
            deleteResource: this.deleteResource
        }
    },
    methods: {
        selectTab(tab) {
            this.selectedTab = tab;
        },
        addResource(resource){
            this.storedResources.unshift(resource);
            this.selectedTab = 'stored-resource';
        },
        deleteResource(id){
            const resourceIndex = this.storedResources.findIndex(r => r.id === id);
            this.storedResources.splice(resourceIndex, 1);
        }
    }
};
</script>