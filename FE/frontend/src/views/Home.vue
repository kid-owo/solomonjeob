<template>
  <AppHeader></AppHeader>
  <AppBanner/>
  <!-- <section>
    <div>
      <img src="@/assets/interview6.jpg" class="img" >

    </div> -->
    <!-- <TTS></TTS> -->
    <!-- <img src="@/assets/interview3.jpg" class="img">
    <img src="@/assets/interview2.jpg" class="img"> -->
  <!-- </section> -->

</template>

<script>
import AppHeader from '@/components/common/AppHeader.vue';
// import TTS from '@/components/interview/tts.vue';
import AppBanner from '@/views/AppBanner.vue';
export default {

  name: 'Home',
  components: {
      AppHeader,
      // TTS,
      AppBanner,


  },
  methods: {

  }
};
</script>

<style scoped>
  /* 캐러셀(이미지슬라이드) 이미지 크기변경 */
  /* .carousel-inner{
    width:100%;
    height:790px; /* 이미지 높이 변경 */
  /* }  */
  /* .carousel-item{
    width: 100%;
    height:100%;
  } */
  /* .d-block {
    display:block;
    width: 100%;
    height: 100%;
  } */
/* body{
  margin:0;
  height: 2000px;
  background: black;
} */
/* section{
  height: 100vh;
   background-size: cover; 
  background-repeat: no-repeat
} */
/* .img {
  width:100%;
  height:100%;
  background: linear-gradient(to top, rgb(95, 92, 92), transparent);
  position:relative;


}
section:before{
  content: '';
  background: linear-gradient(to top, rgb(95, 92, 92), transparent);
  position: absolute;
  left: 0;
  height: 100%;
  width: 100%;
} */

/* .text {
  position:absolute;
  top:40px;
  left:80px;
} */

</style>
