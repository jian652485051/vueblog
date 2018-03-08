<template>
  <div class="admin-tags container">
    <div class="tags-input" v-if="isEdit">
      <el-input v-model="tag.name" @keyup.enter.native="edit"></el-input>
      <el-button type="primary" @click="edit">登 录</el-button>
    </div>
    <ul class="tags-list">
      <li class="list-item" v-for="(tag, index) in tags" :key="index">
        <p class="item-title"><nuxt-link :to="'/tags/'+tag.id">{{tag.name}}</nuxt-link></p>
        <p class="item-date">{{tag.updatedAt | formatDate('yyyy-MM-dd')}}</p>
        <p class="item-del"><a @click="delTag(tag)">删除</a></p>
        <p class="item-edit"><a @click="editTag(tag)">编辑</a></p>
      </li>
    </ul>
    <Tip ref="tip"></Tip>
  </div>
</template>
<script>
export default {
  middleware: 'auth',
  async asyncData({store}) {
    let data = await store.dispatch('TAGS')
    if(data.success) {
      return {
        tags: data.data
      }
    } else {
      return {
        tags: []
      }
    }
  },
  data () {
    return {
      tag:{},
      isEdit: false
    }
  },

  methods: {
    delTag(tag) {
       this.$store.dispatch('DELETE_TAG', tag.id).then((data) => {
        // console.log(data)
        if(data.success) {
         this.$refs.tip.openTip('标签删除完成')
         this.$store.dispatch('TAGS').then((data) => {
           this.tags = data.data
         })
        }
      })
    },

    editTag(tag) {
      this.isEdit = true
      this.tag = tag
    },

    edit() {
      this.isEdit = false
      this.$store.dispatch('UPDATE_TAG', this.tag).then((data) => {
        if(data.success) {
          this.$refs.tip.openTip('标签更新完成')
          this.$store.dispatch('TAGS').then((data) => {
           this.tags = data.data
         })
        }
      })
    }
  }
}

</script>
<style lang="postcss">
.tags-input {
  & .el-input { width:200px; margin-right:15px;}
  & .el-input,
  & .el-button {
     vertical-align:top;
  }
}
</style>