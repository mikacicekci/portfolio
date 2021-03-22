<template>
  <div id="container">
      <component v-bind:is="selectedComponent"></component>
  </div>
</template>

<script>

import WaveAnimation from '@/components/WaveAnimation.vue'
import Snake from '@/components/Snake.vue'
import Info from '@/components/Info.vue'
import Curriculum from '@/components/Curriculum.vue'
import Contact from '@/components/Contact.vue'

export default {
  name: 'Container',
  components: {
    WaveAnimation,
    Snake,
    Curriculum,
    Info,
    Contact
  },
  props: ["selectedComponent"]
}

</script>

<style>

#container {
  /*border-top:    1px solid  #000000;
  border-bottom: 1px solid #000000;*/
  width: 100%;
  height: 100%;
}

</style>