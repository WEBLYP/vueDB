<template>
  <div class="hello">
    <van-list
      v-model="loading"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <div v-for="(item, index) in list" :key="index">
        <img :src="item.images.large" alt="">
        <p>{{item.title}}--{{item.rating.average}}</p>
      </div>
    </van-list>
  </div>
</template>

<script>
import { List, Cell, CellGroup } from 'vant'
import Vue from 'vue'
Vue.use(Cell).use(CellGroup).use(List)
export default {
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
      page: 1,
      count: 5,
      start: 0
    }
  },
  methods: {
    onLoad () {
      const that = this
      // 异步更新数据
      setTimeout(() => {
        this.ajax.get('movie/in_theaters?count=' + this.count + '&start=' + this.start)
          .then(function (res) {
            that.start += 5
            that.loading = false
            that.list = that.list.concat(res.data.subjects)
            if (that.list.length === res.data.total) {
              that.finished = true
            }
          })
          .catch(function (error) {
            console.log(error)
          })
      }, 50)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
