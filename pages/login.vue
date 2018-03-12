<template>
  <div class="login container">
    <el-input v-model="user.username" placeholder="用户名"></el-input>
    <el-input v-model="user.password" placeholder="密码" type="password" @keyup.enter.native="login"></el-input>
    <el-button type="primary" @click="login">登 录</el-button>
    <Tip ref="tip"></Tip>
  </div>
</template>
<script>
export default {
  data() {
    return {
      user:{}
    }
  },
  head () {
    return {
      title: '登录 - VueBlog'
    }
  },
  methods: {
    login () {
      if(!this.user.username || !this.user.password) {
        return
      }
      this.$store.dispatch('LOGIN', this.user).then(data => {
        if(data.success) {
          this.$router.push('/admin/publish')
        } else {
          this.$refs.tip.openTip('用户名或密码不正确')
        }
      })
    }
  }
}
</script>
<style lang="postcss">
.login { 
  max-width:400px; 
  margin-left:auto; 
  margin-right:auto;
  & .el-input {
    margin-bottom:15px;
  }
  & .el-button {
    display:block;
    width:100%;
  }
}
</style>