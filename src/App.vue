<template>
  <div style="height: 100%">
    <h1>Mikail Cicekciler</h1>
    <Container :selectedComponent="selected.component" style="height: calc(100% - 162px)"/>
    <Menu ref="menu" @updateSelected="updateSelected" style="height: 62px; margin: 10px 0px"/>
  </div>
</template>

<script>

import Menu from './components/Menu.vue'
import Container from './components/Container.vue'

export default {
  name: 'App',
  components: {
    Container,
    Menu
  },
  data() {
    return {
      selected: "",
      items: [{
        label: "About me",
        component: "Info"
      }, {
        label: "Curriculum",
        component: "Curriculum"
      }, {
        label: "Home",
        component: "WaveAnimation"
      }, {
        label: "Contact",
        component: "Contact"
      }, {
        label: "Snake",
        component: "Snake"
      }
    ]
    }
  },
  methods: {
    updateSelected(selected) {
      this.selected = selected;
    }
  }
}

</script>

<style>
* {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
}

body, html {
  height: 100%;
  background-image: url('../public/Firefox_wallpaper.png');
  background-repeat: no-repeat;
  background-size: cover;
}

h1 {
  height: 80px;
  font-family: "Lucida Console", Monaco, monospace;
  font-weight: bold;
  text-align: center;
  color: white;
  vertical-align: middle;
  font-size: 50px;
  line-height: 80px;
}

</style>