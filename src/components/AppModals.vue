<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'
import { X, Trash2 } from 'lucide-vue-next'

const props = defineProps<{
  showSave: boolean
  showDelete: boolean
}>()

const emit = defineEmits<{
  (e: 'update:showSave', value: boolean): void
  (e: 'update:showDelete', value: boolean): void
  (e: 'confirm-save', name: string): void
  (e: 'confirm-delete'): void
}>()

const localCustomName = ref('')
const nameInputRef = ref<HTMLInputElement | null>(null)

// Focus input when save modal opens
watch(() => props.showSave, async (isShowing) => {
  if (isShowing) {
    localCustomName.value = ''
    await nextTick()
    if (nameInputRef.value) {
      nameInputRef.value.focus()
    }
  }
})

const handleSave = () => {
  emit('confirm-save', localCustomName.value)
}

const cancelSave = () => {
  emit('update:showSave', false)
}

const handleDelete = () => {
  emit('confirm-delete')
}

const cancelDelete = () => {
  emit('update:showDelete', false)
}
</script>

<template>
  <!-- Custom Save Modal Overlay -->
  <div v-if="showSave" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-bg-dark/80 backdrop-blur-sm transition-opacity">
    <div class="bg-surface border border-slate-700/50 rounded-2xl shadow-2xl p-6 w-full max-w-sm relative transform scale-100 transition-transform">
      <button @click="cancelSave" class="absolute top-4 right-4 text-slate-500 hover:text-white transition-colors">
        <X class="w-5 h-5" />
      </button>
      <h2 class="text-xl font-bold text-white mb-4">儲存網址</h2>
      <p class="text-slate-400 text-sm mb-4">請為這筆網址輸入自定義名稱，若留空將預設使用擷取的網址內容。</p>
      
      <input 
        ref="nameInputRef"
        v-model="localCustomName" 
        @keyup.enter="handleSave"
        type="text" 
        placeholder="例如：我的個人網站" 
        class="w-full bg-slate-900 border border-slate-700 rounded-lg py-3 px-4 text-white placeholder-slate-500 focus:outline-none focus:border-brand-cyan focus:ring-1 focus:ring-brand-cyan mb-6"
      />
      
      <div class="flex space-x-3">
        <button @click="cancelSave" class="flex-1 py-2 px-4 rounded-lg text-slate-300 bg-slate-800 hover:bg-slate-700 transition-colors">
          取消
        </button>
        <button @click="handleSave" class="flex-1 py-2 px-4 rounded-lg font-bold text-white bg-gradient-to-r from-brand-cyan to-blue-500 hover:from-cyan-400 hover:to-blue-400 shadow-md hover:shadow-cyan-500/25 transition-all">
          確認儲存
        </button>
      </div>
    </div>
  </div>

  <!-- Custom Delete Confirmation Modal Overlay -->
  <div v-if="showDelete" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-bg-dark/80 backdrop-blur-sm transition-opacity">
    <div class="bg-surface border border-red-500/30 rounded-2xl shadow-2xl p-6 w-full max-w-sm relative transform scale-100 transition-transform text-center">
      <div class="inline-flex items-center justify-center w-12 h-12 rounded-full bg-red-400/10 text-red-400 mb-4">
        <Trash2 class="w-6 h-6" />
      </div>
      <h2 class="text-xl font-bold text-white mb-2">確認刪除</h2>
      <p class="text-slate-400 text-sm mb-8">您確定要刪除這筆儲存的網址嗎？此動作無法復原。</p>
      
      <div class="flex space-x-3">
        <button @click="cancelDelete" class="flex-1 py-2 rounded-lg text-slate-300 bg-slate-800 hover:bg-slate-700 transition-colors font-medium">
          取消
        </button>
        <button @click="handleDelete" class="flex-1 py-2 rounded-lg font-bold text-white bg-red-500 hover:bg-red-400 shadow-md shadow-red-500/20 transition-all">
          確定刪除
        </button>
      </div>
    </div>
  </div>
</template>
