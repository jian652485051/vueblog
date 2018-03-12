<template>
  <div class="archives container">

    <el-card class="box-card" v-for="(archive, index) in archives" :key="index">
      <div slot="header" class="clearfix">
        <span>{{ archive.date }}({{ archive.total }})</span>
      </div>
      <div v-for="(article, index) in archive.articles" :key="index" class="text item">
        <nuxt-link :to="'/detail/'+article.id">{{ article.title }}</nuxt-link>
      </div>
    </el-card>

  </div>
</template>
<script>
export default {
  async asyncData({ store }) {
    let data = await store.dispatch('ARCHIVES')
    if (data.success) {
      return {
        archives: data.data
      }
    } else {
      return {
        archives: []
      }
    }
  },
  head () {
    return {
      title: '归档 - VueBlog'
    }
  }
}

</script>

<style lang="postcss">
.clearfix:before,  .clearfix:after { display: table; content: ""; }
.clearfix:after { clear: both }
.archives {
  & .box-card { 
    width: auto; 
    margin:15px 0 0;
    & .text { font-size: 14px; }
    & .item { margin-bottom: 18px; }
  }
}
</style>