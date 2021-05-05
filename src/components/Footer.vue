<template>
  <footer class="footer">
    <div class="content">
      <p>
        © 2021
        <a href="https://jonbgua.com" target="_blank" rel="noopener noreferrer">
          Jonbgua
        </a>
        <span style="float: right">
          <router-link to="/help" v-if="this.$route.path !== '/help'">帮助</router-link>
          <router-link to="/" v-if="this.$route.path === '/help'">返回首页</router-link>
        </span>
      </p>
    </div>
  </footer>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style lang="stylus">
footer {
  margin-top: 30px;
  padding-top: 2rem !important;
  padding-bottom: 2rem !important;
}
</style>