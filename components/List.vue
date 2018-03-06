<template>
  <div class="list">

  <el-card class="box-card" v-for="(article, index) in articles" :key="index">
    <div slot="header" class="clearfix">
      <span><nuxt-link class="title-link" :to="'/detail/' + article.id">{{ article.title }}</nuxt-link></span>
    </div>
    <div class="text">
      {{ article.content | cutString(180) }}
    </div>
  </el-card>

  </div>
</template>
<script>
export default {
  props: {
    articles: {
      type: Array,
      default: []
    }
  }
}

</script>
<style lang="postcss">
.list { padding-bottom:20px;}
.text { font-size: 14px; }
.item { margin-bottom: 18px; }
.clearfix:before,  .clearfix:after { display: table; content: ""; }
.clearfix:after { clear: both }
.box-card { width: auto; margin:15px 15px 0;}
</style>
