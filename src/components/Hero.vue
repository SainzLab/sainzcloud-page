<template>
  <section 
    class="relative w-full h-screen flex items-center justify-center overflow-hidden bg-black perspective-container"
    @mousemove="handleMouseMove"
  >
    
    <div class="absolute inset-0 z-0 bg-black">
      <div class="absolute w-full h-full bg-[radial-gradient(circle_at_center,_var(--tw-gradient-stops))] from-gray-900 via-[#050505] to-black opacity-90 animate-pulse-slow"></div>
      <div class="absolute inset-0 bg-[linear-gradient(to_right,#facc151a_1px,transparent_1px),linear-gradient(to_bottom,#facc151a_1px,transparent_1px)] bg-[size:4rem_4rem] [mask-image:radial-gradient(ellipse_80%_60%_at_50%_0%,#000_70%,transparent_100%)] animate-grid-move mix-blend-screen"></div>
      <div class="absolute top-10 left-0 w-full h-24 opacity-5 overflow-hidden transform -skew-y-3">
         <div class="w-[200%] h-full bg-[repeating-linear-gradient(45deg,transparent,transparent_20px,#facc15_20px,#facc15_40px)] animate-scroll-left"></div>
      </div>
      <div class="absolute top-[-50%] left-[-20%] w-[1000px] h-[1000px] bg-gradient-to-b from-yellow-500/10 to-transparent transform rotate-45 animate-spotlight origin-top-left pointer-events-none blur-3xl"></div>
      <div class="absolute top-[-50%] right-[-20%] w-[1000px] h-[1000px] bg-gradient-to-b from-yellow-500/10 to-transparent transform -rotate-45 animate-spotlight-reverse origin-top-right pointer-events-none blur-3xl"></div>
    </div>

    <div class="absolute inset-0 pointer-events-none z-10 overflow-hidden perspective-3d">
        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] border border-yellow-500/10 rounded-full animate-spin-slow-custom opacity-30"></div>
        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[500px] h-[500px] border-2 border-dashed border-yellow-500/20 rounded-full animate-spin-reverse-custom opacity-40"></div>
        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[400px] h-[400px] border-t-2 border-b-2 border-yellow-500/30 rounded-full animate-spin-slow-custom opacity-50"></div>
        
        <div class="absolute top-1/3 left-[10%] opacity-20 animate-float-rotate-slow">
            <svg width="100" height="100" viewBox="0 0 100 100" fill="none" stroke="#facc15" stroke-width="1"><path d="M20 20 L80 20 L80 80 L20 80 Z" /><path d="M30 30 L70 30 L70 70 L30 70 Z" /><path d="M20 20 L30 30 M80 20 L70 30 M80 80 L70 70 M20 80 L30 70" /></svg>
        </div>
        <div class="absolute bottom-1/4 right-[10%] opacity-10 animate-float-rotate-reverse">
             <svg width="150" height="150" viewBox="0 0 100 100" fill="none" stroke="white" stroke-width="0.5"><polygon points="50 5, 90 25, 90 75, 50 95, 10 75, 10 25" /><circle cx="50" cy="50" r="20" /><line x1="50" y1="5" x2="50" y2="30" /><line x1="50" y1="95" x2="50" y2="70" /></svg>
        </div>
        <div v-for="n in 10" :key="'fall'+n" class="absolute text-[10px] font-mono text-yellow-500/40 animate-fall" :style="getParticleStyle(n)">{{ getRandomHex() }}</div>
        <div v-for="n in 15" :key="'ember'+n" class="absolute w-[2px] h-[2px] bg-yellow-400 rounded-full animate-rise" :style="getEmberStyle(n)"></div>
    </div>

    <div class="hidden md:block absolute top-1/3 left-8 w-48 h-64 overflow-hidden mask-gradient-y opacity-40 z-10 pointer-events-none">
        <div class="animate-scroll-up text-[10px] font-mono text-yellow-600 leading-tight">
            <div v-for="(log, i) in logs" :key="i">> {{ log }}</div>
            <div v-for="(log, i) in logs" :key="'dup'+i">> {{ log }}</div>
        </div>
    </div>
    
    <div class="fixed w-8 h-8 border border-yellow-400 rounded-full pointer-events-none z-50 transition-transform duration-75 mix-blend-difference hidden md:block" :style="{ left: cursorX + 'px', top: cursorY + 'px', transform: 'translate(-50%, -50%)' }"><div class="absolute top-1/2 left-1/2 w-1 h-1 bg-red-500 rounded-full transform -translate-x-1/2 -translate-y-1/2"></div></div>

    <div class="relative z-20 text-center transform scale-110 md:scale-100 mix-blend-screen">

      <div class="relative group mt-2 cursor-pointer" @mouseenter="startScramble" @mouseleave="stopScramble">
        <div class="absolute -inset-10 bg-yellow-500/20 blur-[4rem] opacity-30 animate-flicker transition-all duration-300 group-hover:bg-red-500/30 group-hover:opacity-60"></div>
        
        <h1 
            class="glitch-wrapper font-cyber text-7xl md:text-9xl font-black text-white relative inline-block tracking-tighter uppercase transition-transform duration-200"
            :class="{ 'glitch-active': isHovering }"
            :data-text="displayText">
          {{ displayText }}
        </h1>
        
        <div class="mt-6 flex flex-col items-center gap-1">
            <p class="text-gray-400 font-mono text-xs tracking-[0.5em] uppercase transition-colors group-hover:text-yellow-400">
              {{ isHovering ? 'DECRYPTING...' : 'SECURE ENVIRONMENT' }}
            </p>
            <div class="w-24 h-1 bg-gray-800 rounded-full overflow-hidden">
                <div class="h-full bg-yellow-400 w-full animate-loading-bar" :class="{'animate-pulse bg-red-500': isHovering}"></div>
            </div>
        </div>

        <div class="absolute -top-4 -left-4 w-4 h-4 border-t-2 border-l-2 border-yellow-400 transition-all duration-300 group-hover:w-8 group-hover:h-8 group-hover:border-red-500 group-hover:-top-6 group-hover:-left-6"></div>
        <div class="absolute -bottom-4 -right-4 w-4 h-4 border-b-2 border-r-2 border-yellow-400 transition-all duration-300 group-hover:w-8 group-hover:h-8 group-hover:border-red-500 group-hover:-bottom-6 group-hover:-right-6"></div>
      </div>
      
      <div class="mt-16 opacity-0 animate-[fadeInUp_1s_ease-out_1s_forwards]">
         <button class="px-10 py-4 border-2 border-yellow-400 text-yellow-400 hover:bg-yellow-400 hover:text-black transition-all duration-300 font-cyber tracking-[0.2em] font-bold uppercase text-sm relative overflow-hidden group shadow-[0_0_20px_rgba(250,204,21,0.2)]">
            <span class="absolute w-0 h-full bg-white/40 left-0 top-0 group-hover:w-full transition-all duration-300 ease-out -skew-x-12 origin-left"></span>
            <span class="relative z-10 flex items-center gap-2">
                <span class="text-xl leading-none pb-1">»</span> INITIALIZE
            </span>
         </button>
      </div>
    </div>

    <div class="absolute bottom-0 w-full h-40 bg-gradient-to-t from-black via-black to-transparent z-20"></div>
    <div class="absolute inset-0 z-30 pointer-events-none bg-[url('https://grainy-gradients.vercel.app/noise.svg')] opacity-25 mix-blend-overlay"></div>
    <div class="absolute inset-0 z-30 pointer-events-none bg-[linear-gradient(rgba(0,0,0,0)_50%,rgba(0,0,0,0.4)_50%)] bg-[length:100%_4px]"></div>

    <div class="absolute bottom-6 w-full flex justify-between px-10 text-[10px] font-mono text-yellow-600/60 uppercase tracking-widest z-40">
        <div class="flex flex-col text-left">
            <span class="animate-pulse">CPU: 98% // MEM: OVERLOAD</span>
            <span>ZONE: BLACKBOX-01</span>
        </div>
        <div class="flex flex-col text-right">
             <span>ID: 884-XJ-99</span>
             <span>UPTIME: 99.9999%</span>
        </div>
    </div>

  </section>
</template>

<script>
export default {
  name: "Hero",
  data() {
    return {
      mouseX: 0,
      mouseY: 0,
      cursorX: 0,
      cursorY: 0,
      originalText: "SAINZCLOUD",
      displayText: "SAINZCLOUD",
      letters: "ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890@#$%^&*",
      isHovering: false,
      interval: null,
      logs: [
          "INIT_SYSTEM_KERNEL...", "LOADING_ASSETS...", "BYPASSING_FIREWALL...", "ACCESS_GRANTED_LEVEL_5...", 
          "DECRYPTING_DATA_STREAM...", "PING_REMOTE_HOST: OK", "MEMORY_ALLOCATION: 98%", "WARNING: HIGH_VOLTAGE", 
          "ESTABLISHING_UPLINK...", "EXECUTING_SHELL_SCRIPT...", "SCANNING_PORTS...", "VULNERABILITY_FOUND...", "PATCHING_SYSTEM..."
      ]
    };
  },
  methods: {
    handleMouseMove(event) {
      const x = (event.clientX / window.innerWidth) * 2 - 1;
      const y = (event.clientY / window.innerHeight) * 2 - 1;
      this.mouseX = x;
      this.mouseY = y;
      this.cursorX = event.clientX;
      this.cursorY = event.clientY;
    },

    startScramble() {
      this.isHovering = true;
      let iteration = 0;
      
      clearInterval(this.interval);

      this.interval = setInterval(() => {
        this.displayText = this.originalText
          .split("")
          .map((letter, index) => {
            if(index < iteration) {
              return this.originalText[index];
            }
            return this.letters[Math.floor(Math.random() * 26)];
          })
          .join("");
        
        if(iteration >= this.originalText.length){ 
          clearInterval(this.interval);
        }
        
        iteration += 1 / 3; 
      }, 30); 
    },
    stopScramble() {
      this.isHovering = false;
      this.displayText = this.originalText;
      clearInterval(this.interval);
    },
    
    getParticleStyle(n) { /* ...sama... */ return { left: `${Math.random() * 100}%`, animationDuration: `${3 + Math.random() * 5}s`, animationDelay: `${Math.random() * 5}s`, top: '-20px' }},
    getEmberStyle(n) { /* ...sama... */ return { left: `${Math.random() * 100}%`, animationDuration: `${2 + Math.random() * 3}s`, animationDelay: `${Math.random() * 5}s`, bottom: '-10px' }},
    getRandomHex() { /* ...sama... */ const hex = ['0xFA', 'ERR', '404', 'NULL', 'SYS', '01']; return hex[Math.floor(Math.random() * hex.length)]; }
  }
}
</script>

<style scoped>
@keyframes grid-move { 0% { transform: translateY(0); } 100% { transform: translateY(4rem); } }
.animate-grid-move { animation: grid-move 10s linear infinite; }
@keyframes scroll-left { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }
.animate-scroll-left { animation: scroll-left 10s linear infinite; }
@keyframes float-rotate { 0% { transform: translateY(0) rotate(0deg) rotateX(0deg); } 50% { transform: translateY(-20px) rotate(180deg) rotateX(10deg); } 100% { transform: translateY(0) rotate(360deg) rotateX(0deg); } }
.animate-float-rotate-slow { animation: float-rotate 15s infinite linear; }
.animate-float-rotate-reverse { animation: float-rotate 20s infinite linear reverse; }
@keyframes scan { 0% { transform: translateY(-100vh); opacity: 0; } 50% { opacity: 1; } 100% { transform: translateY(100vh); opacity: 0; } }
.animate-scan { animation: scan 4s cubic-bezier(0.4, 0, 0.2, 1) infinite; }
@keyframes fall { 0% { transform: translateY(-10vh); opacity: 0; } 20% { opacity: 1; } 100% { transform: translateY(110vh); opacity: 0; } }
.animate-fall { animation: fall linear infinite; }
@keyframes flicker { 0%, 100% { opacity: 0.3; } 5% { opacity: 0.4; } 10% { opacity: 0.2; } 15% { opacity: 0.4; } 25% { opacity: 0.2; } 30% { opacity: 0.6; } 35% { opacity: 0.1; } 60% { opacity: 0.3; } 70% { opacity: 0.5; } }
.animate-flicker { animation: flicker 3s infinite steps(10); }
@keyframes loading { 0% { width: 0%; } 50% { width: 70%; } 70% { width: 70%; } 100% { width: 100%; } }
.animate-loading-bar { animation: loading 2s infinite ease-in-out; }
.animate-pulse-fast { animation: pulse 1s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
.animate-pulse-slow { animation: pulse 8s infinite ease-in-out; }
@keyframes spin-custom { from { transform: translate(-50%, -50%) rotate(0deg); } to { transform: translate(-50%, -50%) rotate(360deg); } }
.animate-spin-slow-custom { animation: spin-custom 20s linear infinite; }
.animate-spin-reverse-custom { animation: spin-custom 25s linear infinite reverse; }
@keyframes rise { 0% { transform: translateY(0) scale(1); opacity: 0; } 20% { opacity: 0.8; } 100% { transform: translateY(-80vh) scale(0); opacity: 0; } }
.animate-rise { animation: rise linear infinite; }
@keyframes scroll-up { 0% { transform: translateY(0); } 100% { transform: translateY(-50%); } }
.animate-scroll-up { animation: scroll-up 10s linear infinite; }
@keyframes spotlight { 0% { transform: rotate(45deg); opacity: 0.3; } 50% { transform: rotate(55deg); opacity: 0.6; } 100% { transform: rotate(45deg); opacity: 0.3; } }
@keyframes spotlight-reverse { 0% { transform: rotate(-45deg); opacity: 0.3; } 50% { transform: rotate(-55deg); opacity: 0.6; } 100% { transform: rotate(-45deg); opacity: 0.3; } }
.animate-spotlight { animation: spotlight 8s ease-in-out infinite; }
.animate-spotlight-reverse { animation: spotlight-reverse 10s ease-in-out infinite; }
.mask-gradient-y { mask-image: linear-gradient(to bottom, transparent, black 10%, black 90%, transparent); }

.glitch-wrapper {
  color: white;
  text-shadow: 0 0 20px rgba(250, 204, 21, 0.5);
  transition: transform 0.2s, text-shadow 0.2s;
}

.glitch-active {
    transform: scale(1.05) skewX(-5deg);
    text-shadow: 4px 4px 0px rgba(239, 68, 68, 0.8), -4px -4px 0px rgba(234, 179, 8, 0.8);
}

.glitch-wrapper::before, .glitch-wrapper::after {
  content: attr(data-text);
  position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: black; mix-blend-mode: hard-light;
}

.glitch-wrapper::before {
  left: 2px; text-shadow: -2px 0 #facc15;
  animation: glitch-1 3s infinite linear alternate-reverse;
}
.glitch-wrapper::after {
  left: -2px; text-shadow: 2px 0 white;
  animation: glitch-2 2s infinite linear alternate-reverse;
}

.glitch-active::before {
    text-shadow: -5px 0 #ef4444; 
    animation: glitch-1 0.2s infinite linear alternate-reverse;
    clip-path: inset(0 0 0 0);
}
.glitch-active::after {
    text-shadow: 5px 0 #eab308; 
    animation: glitch-2 0.2s infinite linear alternate-reverse;
}

@keyframes glitch-1 {
  0% { clip-path: inset(20% 0 80% 0); }
  20% { clip-path: inset(60% 0 10% 0); }
  100% { clip-path: inset(10% 0 50% 0); }
}
@keyframes glitch-2 {
  0% { clip-path: inset(10% 0 60% 0); }
  20% { clip-path: inset(80% 0 5% 0); }
  100% { clip-path: inset(30% 0 10% 0); }
}
</style>