<template>
  <div id="quitBtn">
    <el-button type="danger" round @click="logOut()">退出</el-button>
  </div>
</template>

<script>
export default {
  methods: {
    logOut () {
      window.sessionStorage.clear()
      this.$router.push('./login')
    }
  }
}
</script>

<style Lang="less" scoped></style>
