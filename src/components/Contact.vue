<template>
    <background>
    </background>
</template>

<script>

import Background from './Background.vue'

export default {
  name: "Contact",
  data() {
    return {
    }
  },
  components: {
    Background
  },
};
</script>

<style>
#infoContact {
  position: relative;
  float: left;
  width: 35%;
  height: 90%;
  margin-left: 5%;
  margin-top: 5vh;
}

#infoContact img {
  width: 70%;
  margin-left: 15%;
  border-radius: 50%;
  border: 2px solid white;
  /*box-shadow: 0px 0px 15px white;*/
}

#infoDescription {
  position: relative;
  float: left;
  width: 50%;
  height: 90%;
  margin-left: 5%;
  margin-top: 5vh;
  overflow: auto;
}

#infoDescription p {
  color: white;
  font-size: 18px;
  font-weight: 12em;
  font-family: poppins,Helvetica,sans-serif;
  line-height: 1.65em;
}

</style>