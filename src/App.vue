<script setup lang="ts">
import { ref, watch } from 'vue'
import MagicInput from './components/MagicInput.vue'
import ResultCard from './components/ResultCard.vue'
import SavedLinksSidebar from './components/SavedLinksSidebar.vue'
import AppModals from './components/AppModals.vue'

// State
const userInput = ref('')
const isSaving = ref(false)

const savedLinks = ref<{ id: string, name: string; payload: string; timestamp: number }[]>([])

// Load saved links from localStorage on mount
const loadSavedLinks = () => {
  const stored = localStorage.getItem('linksnap_saved_links')
  if (stored) {
    try {
      savedLinks.value = JSON.parse(stored)
    } catch (e) {
      console.error('Failed to parse saved links', e)
    }
  }
}
loadSavedLinks()

// Watch and save to localStorage
watch(savedLinks, (newVal) => {
  localStorage.setItem('linksnap_saved_links', JSON.stringify(newVal))
}, { deep: true })

// Modal States
const showSaveModal = ref(false)
const showDeleteModal = ref(false)
const itemToDelete = ref<string | null>(null)

// Actions
const handleSaveClick = () => {
  if (userInput.value.trim().length === 0) return
  showSaveModal.value = true
}

const confirmSave = (customName: string) => {
  isSaving.value = true
  
  savedLinks.value.unshift({
    id: Date.now().toString(),
    name: customName.trim() || userInput.value.slice(0, 30) + (userInput.value.length > 30 ? '...' : ''),
    payload: userInput.value,
    timestamp: Date.now()
  })
  
  showSaveModal.value = false
  
  setTimeout(() => {
    isSaving.value = false
  }, 1000)
}

const openDeleteModal = (id: string, event: Event) => {
  event.stopPropagation()
  itemToDelete.value = id
  showDeleteModal.value = true
}

const confirmDelete = () => {
  if (itemToDelete.value) {
    savedLinks.value = savedLinks.value.filter(link => link.id !== itemToDelete.value)
  }
  showDeleteModal.value = false
  itemToDelete.value = null
}

const loadPayload = (payload: string) => {
  userInput.value = payload
}
</script>

<template>
  <div class="min-h-[100dvh] bg-bg-dark text-slate-100 p-4 py-8 md:p-8 flex items-start md:items-center justify-center font-sans tracking-wide">
    
    <!-- Bento Grid Container -->
    <div class="w-full max-w-6xl grid grid-cols-1 md:grid-cols-12 gap-6 relative z-10" :class="{ 'blur-sm pointer-events-none transition-all duration-300': showSaveModal || showDeleteModal }">
      
      <!-- 1. The Magic Input (Left Side, 7 columns) -->
      <MagicInput v-model="userInput" />

      <!-- 2. Result Card & Saved Links (Right Side, 5 columns) -->
      <div class="col-span-1 md:col-span-5 flex flex-col space-y-6">
        <ResultCard 
          :user-input="userInput" 
          :is-saving="isSaving" 
          @save="handleSaveClick" 
        />
        
        <SavedLinksSidebar 
          :saved-links="savedLinks" 
          @load="loadPayload" 
          @delete="openDeleteModal" 
        />
      </div>

    </div>

    <!-- Modals Overlay -->
    <AppModals 
      v-model:show-save="showSaveModal"
      v-model:show-delete="showDeleteModal"
      @confirm-save="confirmSave"
      @confirm-delete="confirmDelete"
    />

  </div>
</template>
