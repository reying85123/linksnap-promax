<script setup lang="ts">
import { Trash2 } from 'lucide-vue-next'

const props = defineProps<{
  savedLinks: { id: string, name: string; payload: string; timestamp: number }[]
}>()

const emit = defineEmits<{
  (e: 'load', payload: string): void
  (e: 'delete', id: string, event: Event): void
}>()
</script>

<template>
  <div class="bg-surface/60 backdrop-blur-md border border-slate-700/30 rounded-2xl p-6 flex-1 shadow-lg flex flex-col h-full max-h-[320px]">
    <h3 class="text-xs font-mono text-brand-pink mb-4 tracking-widest border-b border-brand-pink/30 pb-2 flex justify-between">
      儲存的網址
      <span class="text-slate-500 opacity-50">{{ savedLinks.length }} 則</span>
    </h3>
    
    <div v-if="savedLinks.length === 0" class="text-center text-slate-600/70 font-mono text-xs py-4 flex-1 flex items-center justify-center italic">
      目前沒有儲存的網址
    </div>

    <div v-else class="space-y-3 overflow-y-auto overscroll-contain flex-1 pr-2 custom-scrollbar">
      <div v-for="item in savedLinks" :key="item.id" 
            class="font-mono text-xs p-3 bg-slate-800/50 hover:bg-slate-800 border border-slate-700/50 rounded flex justify-between items-center group transition-colors cursor-pointer"
            @click="$emit('load', item.payload)"
        >
        <div class="flex-1 overflow-hidden">
          <div class="text-slate-300 group-hover:text-white transition-colors truncate font-bold mb-1">
            {{ item.name }}
          </div>
          <div class="text-slate-500 truncate text-[10px]">
              {{ item.payload }}
          </div>
        </div>
        
        <div class="flex items-center space-x-2 ml-2">
          <div class="text-[10px] text-brand-cyan/70 opacity-0 group-hover:opacity-100 transition-opacity shrink-0">
            載入
          </div>
          <button 
            @click="(e) => $emit('delete', item.id, e)"
            class="p-1.5 text-slate-500 hover:text-red-400 hover:bg-red-400/10 rounded transition-colors opacity-0 group-hover:opacity-100"
            title="刪除"
          >
            <Trash2 class="w-3.5 h-3.5" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Custom minimal scrollbar for saved links */
.custom-scrollbar::-webkit-scrollbar {
  width: 4px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: rgba(30, 41, 59, 0.5); 
  border-radius: 4px;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: rgba(100, 116, 139, 0.5); 
  border-radius: 4px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: rgba(236, 72, 153, 0.5); 
}
</style>
