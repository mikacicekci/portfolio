<template>
    <background>
      <div id="infoContact">
        <table>
          <tr><img :src="picture"></tr>
          <tr><th>{{ age }}</th></tr>
          <tr><th>{{ residency }}</th></tr>
          <tr><th>{{ number }}</th></tr>
          <tr><th>{{ mail }}</th></tr>
        </table>
      </div>
      <div id="infoDescription">
        <p>
          Hello, <br><br>
          My name is Mikail Cicekciler, I am {{ age }} and I live in Istanbul. I have grown up in this city and inhabit it once more after 6 years spent studying 
          in Marseille in the south of France. I have two separate bachelor degrees, one in computer sciences and one in history of sciences. Since my return in Turkey, I have worked 
          as a software engineer at Airties and I am currently performing an intership at Kasar & Dual, the family textile company. I enjoy coding as a hobby, 
          along with all sorts of short manual projects. I am passionated by music and books and enjoy cinema and travels a lot. My life is dedicated to friendships and 
          to beautiful things. I try to live in a sober way and do my best to fulfill my responsibilities. I operate on linux systems for I believe the open-source philosophy 
          is only valid and socially desirable way. I hope the internet will one day be a free, delocated, sober and elegant place that will fully act as a support to enlighted citizenships. 
          As this is not meant to happen anytime soon nor much of the good things that could be wished for society, I can be defined as a cynical and disapointed human swimming 
          lost in the beautiful chaos of life. <br><br> 
          I wish you a pleasant visit and hope to hear from you! 
        </p>
      </div>
    </background>
</template>

<script>

import Background from './Background.vue'

export default {
  name: "Info",
  data() {
    return {
      age: new Date(Date.now() - new Date(1995,3,21)).getUTCFullYear() - 1970,
      number: "+90 534 714 46 77",
      mail: "cicekcilerm@gmail.com",
      residency: "Istanbul, Turkey",
      picture: require("../../public/photo.jpg")
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