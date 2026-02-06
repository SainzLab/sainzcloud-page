<template>
  <div class="fixed right-8 top-1/2 transform -translate-y-1/2 flex flex-col items-end gap-6 z-50">
    
    <div class="absolute right-[11px] top-0 bottom-0 w-[1px] bg-gray-800 h-full -z-10">
       <div class="absolute top-0 left-0 w-full h-[30%] bg-gradient-to-b from-transparent via-yellow-500 to-transparent opacity-50 animate-scan-line"></div>
    </div>

    <div class="absolute right-[-20px] top-1/2 -translate-y-1/2 w-40 h-40 border border-yellow-500/10 rounded-full border-dashed animate-spin-slow pointer-events-none -z-20"></div>

    <div 
      v-for="(item, index) in navItems" 
      :key="index"
      class="relative group flex items-center justify-end cursor-pointer"
      @click="activeIndex = index"
    >
      
      <div 
        class="absolute right-10 opacity-0 transform translate-x-4 group-hover:opacity-100 group-hover:translate-x-0 transition-all duration-300 pointer-events-none"
      >
        <div class="bg-black/80 border border-yellow-500/30 px-3 py-1 backdrop-blur-sm text-right">
          <span class="block text-[10px] text-gray-400 font-mono tracking-widest">SEQ_{{ item.id }}</span>
          <span class="block text-xs text-yellow-400 font-cyber tracking-wider font-bold whitespace-nowrap">{{ item.label }}</span>
        </div>
        <div class="absolute top-1/2 -right-2 w-2 h-[1px] bg-yellow-500/50"></div>
      </div>

      <div class="relative w-6 h-6 flex items-center justify-center transition-all duration-300">
        
        <div v-if="activeIndex === index" class="absolute inset-0 flex justify-between items-center w-full animate-pulse-fast">
             <span class="text-yellow-500 text-xs font-mono">[</span>
             <span class="text-yellow-500 text-xs font-mono">]</span>
        </div>

        <div 
          class="w-2 h-2 transform rotate-45 transition-all duration-300 border border-yellow-500"
          :class="activeIndex === index ? 'bg-yellow-500 scale-110 shadow-[0_0_10px_#facc15]' : 'bg-black group-hover:bg-yellow-500/50'"
        ></div>
        
      </div>
    </div>

    <div class="absolute -bottom-10 right-1 flex flex-col items-center gap-1 opacity-50">
       <div class="w-[1px] h-4 bg-yellow-500/30"></div>
       <span class="text-[8px] text-yellow-500/50 font-mono writing-vertical">SCROLL</span>
    </div>

  </div>
</template>

<script>
export default {
  name: "SideNav",
  data() {
    return {
      activeIndex: 0,
      navItems: [
        { id: '001', label: 'CORE_SYSTEM' },
        { id: '002', label: 'DATA_GRID' },
        { id: '003', label: 'SERVICES' },
        { id: '004', label: 'PROJECTS' },
        { id: '005', label: 'ACCESS_LOG' },
        { id: '006', label: 'CONTACT' },
      ]
    }
  }
}
</script>

<style scoped>
.writing-vertical {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

@keyframes scan-line {
  0% { top: -20%; opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { top: 120%; opacity: 0; }
}
.animate-scan-line {
  animation: scan-line 4s linear infinite;
}

@keyframes spin-slow {
  from { transform: translateY(-50%) rotate(0deg); }
  to { transform: translateY(-50%) rotate(360deg); }
}
.animate-spin-slow {
  animation: spin-slow 20s linear infinite;
}

.font-cyber {
    font-family: 'Orbitron', sans-serif;
}
</style>