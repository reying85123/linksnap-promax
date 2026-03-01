<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  modelValue: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

const value = computed({
  get: () => props.modelValue,
  set: (val) => emit('update:modelValue', val)
})

const hasInput = computed(() => value.value.trim().length > 0)

// Simple, clear placeholders
const placeholderTexts = [
  "輸入網址...",
  "輸入任意文字以產生 QR Code...",
  "貼上您想要分享的連結..."
]
const activePlaceholder = placeholderTexts[Math.floor(Math.random() * placeholderTexts.length)]
</script>

<template>
  <div class="col-span-1 md:col-span-7 flex flex-col justify-center space-y-6">
    <div>
      <h1 class="text-4xl md:text-5xl font-bold tracking-tight text-white mb-2">
        Link<span class="text-brand-cyan drop-shadow-[0_0_10px_rgba(6,182,212,0.8)]">Snap </span><span class="text-brand-pink text-2xl drop-shadow-[0_0_10px_rgba(236,72,153,0.8)]">v1</span>
      </h1>
      <p class="text-slate-400 text-lg font-light tracking-wide">隨時隨地，即刻產生與分享你的 QR Code。</p>
    </div>

    <!-- Glassmorphism Textarea Container with Neon Focus Flow -->
    <div class="relative group">
      <div class="absolute -inset-0.5 bg-gradient-to-r from-brand-cyan to-brand-pink rounded-2xl blur opacity-30 group-focus-within:opacity-100 transition duration-500 delay-75"></div>
      <div class="relative bg-surface/80 backdrop-blur-xl border border-slate-700/50 rounded-2xl p-6 shadow-2xl focus-within:border-brand-cyan/50 focus-within:shadow-neon-cyan transition-all duration-300 transform group-hover:-translate-y-1">
        <textarea 
          v-model="value"
          :placeholder="activePlaceholder"
          class="w-full h-40 bg-transparent text-xl md:text-2xl text-white placeholder-slate-600 outline-none resize-none font-mono tracking-wide selection:bg-brand-pink/30"
        ></textarea>
        
        <div class="flex justify-between items-center mt-4 border-t border-slate-700/50 pt-4">
          <span class="text-xs font-mono" :class="hasInput ? 'text-brand-cyan animate-pulse' : 'text-slate-500'">
            {{ hasInput ? '準備就緒...' : '等待輸入內容' }}
          </span>
          <span class="text-xs font-mono" :class="hasInput ? 'text-brand-pink' : 'text-slate-500'">
            目前字數：{{ value.length }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>
