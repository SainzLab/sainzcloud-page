<template>
  <div class="relative bg-black min-h-screen text-white overflow-hidden">
    <Hero />
    <SideNav />
  </div>
</template>

<script>
import Hero from './components/Hero.vue'
import SideNav from './components/SideNav.vue'

export default {
  name: 'App',
  components: {
    Hero,
    SideNav
  }
}
</script>