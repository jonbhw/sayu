<template>
  <div id="app">
    <Hero/>
    <router-view/>
    <Footer/>
  </div>
</template>

<script>
import Hero from "@/components/Hero.vue";
import Footer from "@/components/Footer.vue";

export default {
  components: {
    Hero, Footer
  },
};
</script>

<style lang="stylus">
#app {
  font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, PingFang SC, Microsoft YaHei, Source Han Sans SC, Noto Sans CJK SC, WenQuanYi Micro Hei, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>