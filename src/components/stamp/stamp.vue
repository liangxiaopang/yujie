<template>
<transition name="move">
  <div class="stamp" v-show="showFlag" ref='stamp'>
    <div class="stamp-content">
      <div class="stamp-content-image-header">
      	<img :src="stamp.image" alt="" width="375" height="375">
      	<div class="stamp-content-back" @click='hide'>
      		<i class="icon-arrow_lift"></i>
      	</div>
      </div>
      <div class="stamp-content-content">
      	<h1 class="title">{{stamp.name}}</h1>
      	<div class='detail'>
      		<span class="sell-count">月售{{stamp.sellCount}}份</span>
      		<span class="rating">好评率{{stamp.rating}}%</span>
      	</div>
      	<div class="price">
      		<span class="now">现价:${{stamp.price}}元</span><span class="old" v-show="stamp.oldPrice">原价:${{stamp.oldPrice}}</span>
      	</div>
      	<div class="cartcontrol-wrapper">
      	  <cartcontrol :stamp="stamp" @add='addstamp'></cartcontrol>
        </div>
        <div class="buy" v-show="!stamp.count||stamp.count==0" @click.stop.prevent="addFirst">加入购物车</div>
      </div>
      <split v-show="stamp.info"></split>
      <div class="info" v-show="stamp.info">
      	<h1 class="title">商品介绍</h1>
      	<p class="text">{{stamp.info}}</p>
      </div>
      <split></split>
      <div class="rating">
      	<h1 class="title">商品评价</h1>
      	<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="stamp.ratings" @select="selectRating" @toggle="toggleContent"></ratingselect>
      	<div class="rating-wrapper">
      		<ul v-show="stamp.ratings&&stamp.ratings.length">
      			<li v-show="needShow(this.rateType,this.text)" v-for="rating in stamp.ratings" class="rating-item border-1px">
      			  <div class="user">
      			  	<span class="name">{{rating.username}}</span>
      			  	<img :src="rating.avatar" width="12" height="12" class="avatar">
      			  </div>
      			  <div class="time">{{rating.rateTime | formatData}}</div>
      			  <p class="text" v-show="rating.text">
      			  	<span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
      			  </p>
      			</li>
      		</ul>
      		<div v-show="!stamp.ratings||!stamp.ratings.length" class="no-rating">暂无评价	
       		</div>
      	</div>
      </div>
    </div>	
  </div>
</transition>
</template>        
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import cartcontrol from '../cartcontrol/cartcontrol'
  import ratingselect from '../ratingselect/ratingselect'
  import Vue from 'vue'
  import split from '../split/split'
  
  // const POSITIVE = 0
  // const NEGATIVE = 1
  const ALL = 2

  export default {
    props: {
      stamp: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    methods: {
      show () {
        this.showFlag = true
        this.selectType = 1
        this.onlyContent = true
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.stamp, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      },
      selectRating (type) {
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      toggleContent () {
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      hide () {
        this.showFlag = false
      },
      addFirst (event) {
        Vue.set(this.stamp, 'count', 1)
      },
      needShow (type, text) {
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        } else {
          return type === this.selectType
        }
      },
      addstamp (target) {
        this.$emit('add', target)
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .stamp
    position:fixed
    left:0
    top:0
    bottom:48px
    z-index:30
    width:100%
    background:#fff
    &.move-enter-active,&.move-leave-active
      transition:all .2s linear
      transform:translate3d(0,0,0)
    &.move-enter,&.move-leave
      transform:translate3d(100%,0,0)
    .stamp-content-image-header
      position:relative
      width:100%
      height:0
      padding-top:100%
      img
        position:absolute
        top:0
        left:0
        width:100%
        height:100%
      .stamp-content-back
        position:absolute
        left:10px
        top:10px
        .icon-arrow_lift
          display:block
          padding:10px
          font-size:20px
          color:#fff
    .stamp-content-content
      position:relative
      padding:18px
      .title
        line-height:14px
        margin-bottom:8px
        font-size:14px
        font-weight:700
        color:rgb(7,17,27)
      .detail
        margin-bottom:18px
        height:10px
        line-height:10px
        font-size:0
        .sell-count,.rating
          font-size:10px
          color:rgb(147,153,159)
        .sell-count
          margin-right:12px
      .price
        font-weight:700px
        line-height:24px
        .now
          margin-right:8px
          font-size:14px
          color:rgb(240,20,20)
        .old
          text-decoration:line-through
          font-size:10px
          color:rgb(147,153,159)
      .cartcontrol-wrapper
        position:absolute
        right:12px
        bottom:12px
      .buy
        position:absolute
        right:18px
        bottom:18px
        z-index:10
        height:24px
        line-height:24px
        padding:0 12px
        box-sizing:border-box
        font-size:10px
        border-radius:12px
        color:#fff
        background:rgb(0,160,220)
    .info
      padding:18px
      .title
        line-height:14px
        margin-bottom:6px
        font-size:14px
        color:rgb(7,17,27)
      .text
        line-height:24px
        padding:0 8px
        font-size:12px
        color:rgb(77,85,93)
    .rating
      padding-top:18px
      .title
        line-height:14px
        margin-left:18px
        font-size:14px
        color:rgb(7,17,27)
      .rating-wrapper
        padding:0 18px
        .rating-item
          position:relative
          padding:16px 0
          border-1px(rbga(7,17,27,.1))
          .user
            position:absolute
            right:0
            top:16px
            font-size:0
            line-height:12px
            .name
              font-size:10px
              margin-right:6px
              display:inline-block
              vertical-align:top
              color:rgb(147,153,159)
            .avatar
              border-radius:50%
          .time
            line-height:12px
            font-size:1opx
            color:rgb(147,153,159)
            margin-bottom:6px
          .text
            line-height:16px
            font-size:12px
            color:rgb(7,17,27)
            .icon-thumb_down,.icon-thumb_up
              line-height:24px
              margin- right:4px
              font-size:12px
              &.icon-thumb_up
                color:rgb(0,160,220)
              &.icon-thumb_down
                color:rgb(147,153,159)
        .no-rating
          padding:16px 0
          font-size:12px
          color:rgb(147,153,159)











	
</style>