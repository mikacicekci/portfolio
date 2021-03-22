<template>
  <background>
    <div id="curriculumContainer">
      <embed id="pdfViewer" :src="curriculumSrc" type="application/pdf" />
      <div id="pdfList">
        <div v-for="item in items" :key="item" class="pdfItemContainer">
          <div
            :class="[
              'file-icon file-icon-xl',
              'pdfItem',
              active == item ? 'active' : '',
            ]"
            :data-type="item.datatype"
            @click="setActive(item)"
          ></div>
        </div>
      </div>
    </div>
  </background>
</template>

<script>
import Background from "./Background.vue";

export default {
  name: "Curriculum",
  data() {
    return {
      active: undefined,
      curriculumSrc: require("./../assets/cv_mikailcicekciler_english.pdf"),
      items: [
        {
          label: "Français",
          src: require("./../assets/cv_mikailcicekciler_francais.pdf"),
          datatype: "french",
        },
        {
          label: "English",
          src: require("./../assets/cv_mikailcicekciler_english.pdf"),
          datatype: "english",
        },
        {
          label: "Turkçe",
          src: require("./../assets/cv_mikailcicekciler_turkce.pdf"),
          datatype: "turkish",
        },
      ],
    };
  },
  methods: {
    setActive(item) {
      this.active = item;
      this.updateCurriculumSrc(item.src);
    },
    updateCurriculumSrc(src) {
      this.curriculumSrc = src;
    },
  },
  mounted() {
    this.setActive(this.items[0]);
  },
  components: {
    Background,
  },
};
</script>

<style>
#curriculumContainer {
  width: 100%;
  height: 100%;
}

#pdfViewer {
  float: left;
  width: calc(100% - 160px);
  height: 100%;
}

#pdfList {
  float: right;
  width: 160px;
  height: 100%;
  overflow: auto;
  background: white;
}

.pdfItemContainer {
  width: 96px;
  height: 128px;
  margin: 16px 32px;
}

.pdfItem {
  width: 100%;
  height: 100%;
  float: left;
  transition-duration: 1s;
}

.pdfItem:not(.active) {
  background: #8e44ad;
  transform: rotate3d(-2, 1, 0, 0deg);
  -webkit-transform: rotate3d(-2, 1, 0, 0deg);
}

.active {
  background: #f4b400;
  transform: rotate3d(-2, 1, 0, -360deg);
  -webkit-transform: rotate3d(-2, 1, 0, -360deg);
}

.pdfItemContainer:hover .pdfItem:not(.active) {
  background: #f4b400;
  transform: rotate3d(-2, 1, 0, -340deg);
  -webkit-transform: rotate3d(-2, 1, 0, -340deg);
}

/*! fileicon.css v0.1.1 | MIT License | github.com/picturepan2/fileicon.css */
/* Code imported from fileicon.css. MODIFIED! */

.file-icon {
  font-family: Arial, Tahoma, sans-serif;
  font-weight: 300;
  width: 96px;
  height: 128px;
  display: inline-block;
  position: relative;
  border-radius: 4px;
  text-align: left;
  -webkit-font-smoothing: antialiased;
}

.file-icon::before {
  display: block;
  content: "";
  position: absolute;
  top: 0;
  right: 0;
  width: 0;
  height: 0;
  
  border-bottom-left-radius: 4px;
  border-width: 16px;
  border-style: solid;
  border-color: #fff #fff rgba(255, 255, 255, 0.35) rgba(255, 255, 255, 0.35);
}

.file-icon::after {
  display: block;
  content: attr(data-type);
  position: absolute;
  bottom: 0;
  left: 0;
  color: #fff;
  text-transform: lowercase;
  width: 100%;
  font-size: 24px;
  padding: 4px 10px;
  white-space: nowrap;
  overflow: hidden;
}
</style>