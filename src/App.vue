<script setup lang="ts">
</script>

<template>
  <div class="min-h-screen bg-neutral-900 flex items-center justify-center p-4">
    <div class="max-w-md w-full bg-neutral-800 rounded-2xl shadow-xl overflow-hidden border border-neutral-700">
      <div class="p-8 text-center space-y-6">
        <div class="inline-flex items-center justify-center w-16 h-16 rounded-full bg-cyan-500/20 text-cyan-400 mb-2">
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="20 6 9 17 4 12"></polyline>
          </svg>
        </div>
        
        <h1 class="text-3xl font-bold tracking-tight text-white">
          Tailwind CSS v4
        </h1>
        
        <p class="text-neutral-400 text-lg">
          如果這張卡片有深灰背景、青色打勾圖示、並且完美置中，代表 Tailwind 設定成功！🚀
        </p>

        <button class="w-full py-3 px-4 bg-gradient-to-r from-cyan-500 to-blue-500 hover:from-cyan-400 hover:to-blue-400 text-white font-medium rounded-lg transition-all shadow-lg hover:shadow-cyan-500/25 active:scale-[0.98]">
          開始打造 LinkSnap
        </button>
      </div>
    </div>
  </div>
</template>

<style>
/* 這裡不需要其他 CSS，因為我們完全依賴 Tailwind v4 */
</style>
