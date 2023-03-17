<script setup>
import { ref, provide } from 'vue'
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

const tabs = { StoredResources, AddResource }

const selectedTab = ref('StoredResources')

const resourcesList = ref([
  {
    id: 'official-guide',
    title: 'Official Guide',
    description: 'The official Vue.js documemntation.',
    link: 'https://vuejs.org'
  },
  {
    id: 'google',
    title: 'Google',
    description: 'Learn to google.',
    link: 'https://google.rw'
  }
])

const activeTab = (tab) => {
  if (tab == 'AddResource') {
    return selectedTab.value === 'AddResource' ? null : 'flat'
  } else return selectedTab.value === 'StoredResources' ? null : 'flat'
}

const addResourceData = (title, description, link) => {
  const newResource = {
    id: new Date().toISOString(),
    title,
    description,
    link
  }
  console.log(newResource)
  resourcesList.value.unshift(newResource)
  selectedTab.value = 'StoredResources'
}

const removeResource = (resId) => {
  const foundItem = resourcesList.value.find((r) => r.id === resId)
  resourcesList.value.splice(foundItem, 1)
}
provide('resources', resourcesList)
provide('addResourceData', addResourceData)
provide('deleteResource', removeResource)
</script>

<template>
  <base-card>
    <base-button
      v-for="(_, tab) in tabs"
      :key="tab"
      @click="selectedTab = tab"
      :mode="activeTab(tab)"
      >{{ tab === 'StoredResources' ? 'Stored Resources' : 'Add Resource' }}</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="tabs[selectedTab]"></component>
  </keep-alive>
</template>
