<template>
  <div id="app" class="w-full min-h-screen">
    <div class="container px-4 mx-auto text-center py-10">
      <h1 class="title bg-black text-white inline-block px-8 py-2 rounded-xl text-l md:text-3xl">The emoji piano</h1>
      <piano></piano>
    </div>
  </div>
</template>

<script>
import Piano from '@/components/Piano'

export default {
  name: 'App',
  components: {
    Piano
  }
}
</script>

<style lang="postcss" scoped>
#app {
  background: linear-gradient(135deg, #FF9D6C 0%, #BB4E75 100%);
  font-family: 'PT Mono', monospace;
}
</style>