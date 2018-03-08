<template>
  <div class="admin-update container">
    <div class="update-info">
      <h4>修改个人信息</h4>
      <el-input v-model="user.nickname"></el-input>
      <el-input v-model="user.motto"></el-input>
      <el-input v-model="user.email"></el-input>
      <el-button type="primary" @click="updateInfo">确认修改</el-button>
    </div>
    <div class="update-password">
      <h4>修改密码</h4>
      <el-input v-model="oldPassword" placeholder="输入旧密码"></el-input>
      <el-input v-model="newPassword" placeholder="输入新密码"></el-input>
      <el-input v-model="vertifyPassword" placeholder="再次输入新密码"></el-input>
      <el-button type="primary" @click="updatePassword">确认修改</el-button>
    </div>
    <Tip ref="tip"></Tip>
  </div>
</template>
<script>
export default {
  middleware: 'auth',
  async asyncData({ store }) {
    let data = await store.dispatch('ADMIN_INFO')
    if (data.success) {
      return {
        user: data.data
      }
    } else {
      return {
        user: {}
      }
    }
  },

  data() {
    return {
      oldPassword: '',
      newPassword: '',
      vertifyPassword: ''
    }
  },

  methods: {
    updateInfo() {
      this.$store.dispatch('UPDATE_ADMIN', this.user).then((data) => {
        if(data.success) {
          this.$refs.tip.openTip('信息修改完成')
        }
      })
    },
    updatePassword() {
      if (!this.oldPassword || !this.newPassword || !this.vertifyPassword) {
        return
      }
      if (this.newPassword !== this.vertifyPassword) {
        this.$refs.tip.openTip('两次密码不一致！')
        return
      }
      this.$store.dispatch('UPDATE_ADMIN', { oldPassword: this.oldPassword, newPassword: this.newPassword }).then((data) => {
        if(data.success) {
          this.$refs.tip.openTip('密码重置完成')
          // clear token
          this.$store.dispatch('LOGOUT').then(ret => {
            if(ret.success) {
              this.$store.state.token = ''
              this.$router.push('/')
            }
          })
        }
      })
    }
  }
}

</script>

