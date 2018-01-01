<template>
  <div id="app">
    <div class="header">
        <v-header :seller='seller'></v-header>
    </div>
    <div class="tab">
      <div class="tab_item">
          <router-link to="/goods">商品</router-link>
      </div>
     <div class="tab_item">
         <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab_item">
            <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="content">
      <router-view :seller="seller"/>
    </div>
  </div>
</template>

<script>
import Header from './components/header/header'
const ERR_NO = 0
export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  components: {
    'v-header': Header
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      res = res.body
      if (res.errno === ERR_NO) {
        this.seller = res.data
      }
    })
  }
}
</script>

<style  lang='stylus' rel='stylesheet/stylus'>
@import './common/stylus/mixin.styl'
  .tab
    height: 40px
    width: 100%
    display: flex
    border-1px(rgba(7, 17, 27, 0.1))
    .tab_item
      width: (100/3)%
      a
        display: block
        line-height: 40px
        font-size: 14px 
        color: rgb(78,85,93)
        text-align: center
        &.active
          color: rgb(240,20,20)
</style>
