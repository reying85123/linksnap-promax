<script setup lang="ts">
import { computed, ref } from 'vue'
import QrcodeVue from 'qrcode.vue'
import { Download, Share2, Check, Bookmark, BookmarkCheck } from 'lucide-vue-next'
import { useShare } from '@vueuse/core'

const props = defineProps<{
  userInput: string
  isSaving: boolean
}>()

const emit = defineEmits<{
  (e: 'save'): void
}>()

const isDownloading = ref(false)

const hasInput = computed(() => props.userInput.trim().length > 0)
const displayValue = computed(() => hasInput.value ? props.userInput : 'https://example.com/placeholder')

// Reactive wrapper for useShare
const { share, isSupported: isShareSupported } = useShare()

const handleShare = async () => {
  if (!isShareSupported.value) return;
  await share({
    title: 'LinkSnap 產生的 QR Code',
    text: `內容：${props.userInput}`,
    url: props.userInput, // Treat input as URL if possible
  })
}

const handleDownload = () => {
  isDownloading.value = true
  
  // Find the canvas element inside our QrcodeVue component
  const canvas = document.querySelector('.qr-target canvas') as HTMLCanvasElement
  if (canvas) {
    const url = canvas.toDataURL("image/png")
    const a = document.createElement('a')
    a.href = url
    a.download = `linksnap-qr-${Date.now()}.png`
    document.body.appendChild(a)
    a.click()
    document.body.removeChild(a)
  }

  // Reset button state after 2s
  setTimeout(() => {
    isDownloading.value = false
  }, 2000)
}
</script>

<template>
  <!-- QR Code Result Card (Glassmorphism) -->
  <div class="bg-surface/80 backdrop-blur-xl border border-slate-700/50 rounded-2xl p-8 flex flex-col items-center justify-center shadow-xl relative overflow-hidden group">
    <!-- Decorative scanline, intensifies when has input -->
    <div class="absolute inset-0 bg-gradient-to-b from-transparent via-brand-cyan/10 to-transparent h-full w-full opacity-30 transform -translate-y-[200%] animate-[scan_4s_ease-in-out_infinite]"
          :class="{ 'via-brand-cyan/20 animate-[scan_2s_ease-in-out_infinite]': hasInput }"></div>
    
    <div class="text-xs text-brand-cyan/50 font-mono tracking-widest mb-6 absolute top-4 left-6">
      預覽區塊
    </div>
    
    <!-- The Magic QR Block with Breathing animation on input change -->
    <div class="p-4 bg-white/5 rounded-2xl border border-white/10 backdrop-blur-md relative transform transition-all duration-500 mt-6"
          :class="{ 'scale-105 shadow-[0_0_30px_rgba(6,182,212,0.3)] border-brand-cyan/30': hasInput, 'scale-100 opacity-60 grayscale cursor-not-allowed': !hasInput }">
        <div class="qr-target relative" :key="displayValue">
          <!-- Wrap QrcodeVue to force re-render/animation on key change -->
          <QrcodeVue 
            :value="displayValue" 
            :size="200"
            level="H"
            foreground="#F8FAFC"
            background="transparent"
            class="block transition-opacity duration-300"
          />
        </div>
    </div>

    <!-- Action Buttons -->
    <div class="flex space-x-3 mt-8 w-full px-2 relative z-10 transition-all duration-300" :class="{ 'opacity-50 pointer-events-none': !hasInput }">
      
      <button 
        @click="handleDownload"
        class="flex-1 py-3 px-2 bg-slate-800 hover:bg-slate-700 border border-slate-600 hover:border-brand-cyan text-white text-sm font-bold tracking-wide rounded-xl transition-all shadow-lg focus:outline-none flex justify-center items-center gap-2 group/btn whitespace-nowrap"
        :class="{ 'scale-95 bg-brand-cyan/20 border-brand-cyan text-brand-cyan': isDownloading }"
      >
        <Check v-if="isDownloading" class="w-4 h-4 shrink-0" />
        <Download v-else class="w-4 h-4 shrink-0 group-hover/btn:text-brand-cyan transition-colors" />
        <span>{{ isDownloading ? '下載完成' : '下載圖片' }}</span>
      </button>

      <button 
        @click="$emit('save')"
        class="flex-1 py-3 px-2 bg-slate-800 hover:bg-slate-700 border border-slate-600 hover:border-brand-pink text-white text-sm font-bold tracking-wide rounded-xl transition-all shadow-lg focus:outline-none flex justify-center items-center gap-2 group/btn whitespace-nowrap"
        :class="{ 'scale-95 bg-brand-pink/20 border-brand-pink text-brand-pink': isSaving }"
      >
        <BookmarkCheck v-if="isSaving" class="w-4 h-4 shrink-0" />
        <Bookmark v-else class="w-4 h-4 shrink-0 group-hover/btn:text-brand-pink transition-colors" />
        <span>{{ isSaving ? '已儲存！' : '儲存網址' }}</span>
      </button>
      
      <button 
        v-if="isShareSupported"
        @click="handleShare"
        class="p-3 shrink-0 bg-slate-800 hover:bg-slate-700 border border-slate-600 hover:border-brand-pink text-slate-300 hover:text-brand-pink rounded-xl transition-all focus:outline-none focus:ring-2 focus:ring-brand-pink/50"
        title="分享"
      >
          <Share2 class="w-5 h-5" />
      </button>
    </div>
  </div>
</template>

<style>
@keyframes scan {
  0% { transform: translateY(-200%); }
  100% { transform: translateY(200%); }
}
</style>
