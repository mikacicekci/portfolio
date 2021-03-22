<template>
  <div id="outsideContainer">
    <div id="insideContainer">
      <slot></slot>
    </div>
  </div>
</template>

<script>

export default {
  name: "Background"
};
</script>

<style>
#outsideContainer {
  width: 80%;
  height: 80%;
  position: relative;
  padding-left: 10%;
  padding-top: 8vh;
  color: white;
  font-size: 18px;
  font-weight: 12em;
  font-family: poppins,Helvetica,sans-serif;
  line-height: 1.65em;
}

#insideContainer {
  width: 100%; 
  height: 100%; 
  background-color: rgba(0, 0, 0, 0.8);
}
</style>