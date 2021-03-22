<template>
  <div id="footer">
    <table style="height: auto; width: 100%; text-align: center">
      <tr>
        <td>
          <div
            v-for="(item, index) in items"
            :key="item"
            style="display: inline-block; position: relative"
          >
            <div class="navItem navSeparator" v-if="index">-</div>
            <div
              :class="['navItem navClickable', active == item ? 'navActive' : '']"
              v-text="item.label"
              @click="click(item, index * 2)"
            />
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import $ from "jquery";

export default {
  name: "Menu",
  data() {
    let maxFontSize = 32;
    let navFontSize = 16;
    let nbDegrades = 4;
    return {
      items: [
        {
          label: "About me",
          component: "Info",
        },
        {
          label: "Curriculum",
          component: "Curriculum",
        },
        {
          label: "Home",
          component: "WaveAnimation",
        },
        {
          label: "Contact",
          component: "Contact",
        },
        {
          label: "Snake",
          component: "Snake",
        },
      ],
      active: undefined,
      curPos: 0,
      nbDegrades: nbDegrades,
      maxFontSize: maxFontSize,
      navFontSize: navFontSize,
      navSpace: parseInt((maxFontSize - navFontSize) / nbDegrades),
    };
  },
  mounted() {
    let middlePos = Math.floor(this.items.length / 2);
    this.click(this.items[middlePos], middlePos * 2);
  },
  methods: {
    setSize(curPos, index, val) {
      let that = this;
      setTimeout(function () {
        for (
          var j = -that.nbDegrades * val;
          j != (that.nbDegrades + 1) * val;
          j += val
        ) {
          if (curPos + j < 0 || curPos + j >= that.items.length * 2) continue;
          $($(".navItem")[curPos + j]).animate(
            {
              fontSize: that.maxFontSize - Math.abs(j) * that.navSpace + "px",
            },
            50
          );
        }
        curPos += val;
        if (curPos != index + val) {
          that.setSize(curPos, index, val);
        } else {
          that.curPos = index;
        }
      }, 25);
    },
    animate(index) {
      if (index == this.curPos) {
        return;
      } else if (index > this.curPos) {
        this.setSize(this.curPos, index, 1);
      } else {
        this.setSize(this.curPos, index, -1);
      }
    },
    click(item, index) {
      this.active = item
      this.$emit("updateSelected", item);
      this.animate(index);
    },
  },
};
</script>

<style scoped>
#footer {
  position: relative;
  float: left;
  width: 100%;
  height: 100%;
  text-align: center;
  background-color: rgb(255, 255, 255);
}

.navItem {
  font-family: "Lucida Console", Monaco, monospace;
  font-weight: bold;
  text-align: center;
  vertical-align: middle;
  font-size: 16px;
  /*margin: 0 2px;*/
  height: 50px;
  line-height: 50px;
  position: relative;
  float: left;
  color: black;
  -moz-user-select: -moz-none;
  -khtml-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /*clear: both;*/
}

.navSeparator {
  width: 50px;
}

.navClickable {
  width: auto;
  cursor: pointer;
}

.navActive, .navClickable:hover {
  border-bottom: 2px solid black;
}

#gamesnav {
  display: inline-block;
  position: relative;
}
</style>