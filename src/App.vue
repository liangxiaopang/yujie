<template>
  <div id="app">
      <hrs :seller="seller"></hrs>
      <div class='tab border-1px'>
      <div class='tab-item'>
        <router-link to="/goods" class='links'>商品</router-link>
      </div>
      <div class='tab-item'>
        <router-link to="/ratings" class='links'>评论</router-link>
      </div>
      <div class='tab-item'>
        <router-link to="/seller" class='links'>关于钰杰</router-link>
      </div>
      </div>
      <router-view :seller='seller'></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
import header from './components/header/header.vue'
import axios from 'axios'
const ERR_NO = 0
export default {
  created () {
    axios.get('/api/seller').then((response) => {
      response = response.data
      if (response.errno === ERR_NO) {
        this.seller = response.data
      }
    })
  },
  data () {
    return {
      seller: {}
    }
  },
  components: {
    'hrs': header
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>                          
  @import './common/stylus/mixin.styl'
  #app
    .tab
      display:flex
      width:100%
      height:40px
      line-height:40px
      border-1px(rgba(7,17,27,.2))
      .tab-item
        flex:1
        text-align:center
        & > .links
          display: block
          text-decoration:none
          font-size:14px
          color:rgb(77,85,93)
          &.active
            color: rgb(240,20,20)


</style>
