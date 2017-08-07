<template>
<div>
	<div class="goods">
		<div class="menu-wrapper" ref='menuWrapper'>
      <ul>
        <li v-for='(item,index) in goods' class='menu-item'  @click='stampscroll(index,$event)' :class="{'current':currentIndex==index}">
          <span class='text border-1px' >
            <span v-show='item.type>0' class='icon' :class='classMap[index]'></span>{{item.name}}
          </span>
        </li>
      </ul>
		</div>
		<div class="stamps-wrapper" ref='stampsWrapper'>
			<ul>
        <li v-for='item in goods' class='stamp-list' ref='stamplist'>
          <h1 class='stamp-title'>{{item.name}}</h1>
          <ul>
            <li @click="selectstamp(stamp,$event)" v-for='stamp in item.stamps' class='stamp-item border-1px'>
              <div class='icon'><img :src="stamp.icon" width='57' height='57'></div>
              <div class='stamp-content'>
                <h2 class='stamp-name'>{{stamp.name}}</h2>
                <p class='stamp-desc'>{{stamp.description}}</p>
                <div class="stamp-extra">
                  <span class='stamp-count'>已售{{stamp.sellCount}}份</span>
                  <span>好评率{{stamp.rating}}%</span>
                </div>
                <div class="stamp-price">
                  <span class='now'>现价:${{stamp.price}}</span>
                  <span v-show='stamp.oldPrice' class='old'>原价:${{stamp.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol @add='addstamp' :stamp="stamp"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>   
      </ul>
		</div>
    <shopcart ref='shopcart' :selectstamps='selectstamps' :deliveryPrice='seller.deliveryPrice' :minPrice='seller.minPrice'></shopcart>
	</div>
  <stamp :stamp="selectedstamp" ref="stamped" @add="addstamp"></stamp>
</div>
</template>

<script type="text/ecmascript-6">
import axios from 'axios'
import BScroll from 'better-scroll'
import shopcart from '../shopcart/shopcart'
import cartcontrol from '../cartcontrol/cartcontrol'
import stamp from '../stamp/stamp'
const ERR_NO = 0
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listheight: [],
      scrollY: 0,
      selectedstamp: {},
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.stampsScroll = new BScroll(this.$refs.stampsWrapper, {
        click: true,
        probeType: 3
      })
      this.stampsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    stampscroll (index, event) {
      if (!event._constructed) {
        return false
      }
      let stamplist = this.$refs.stamplist
      let el = stamplist[index]
      this.stampsScroll.scrollToElement(el, 300)
    },
    selectstamp (stamp, event) {
      if (!event._constructed) {
        return false
      }
      this.selectedstamp = stamp
      this.$refs.stamped.show()
    }
  },
  computed: {
    selectstamps () {
      let stamps = []
      this.goods.forEach((good) => {
        good.stamps.forEach((stamp) => {
          if (stamp.count) {
            stamps.push(stamp)
          }
        })
      })
      return stamps
    }
  },
  created () {
    axios.get('/api/goods').then((response) => {
      response = response.data
      if (response.errno === ERR_NO) {
        this.goods = response.data
        this.$nextTick(() => {
          this._initScroll()
          // this._calculateHeight()
        })
      }
    })
  },
  components: {
    shopcart,
    cartcontrol,
    stamp
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.goods
  position:absolute
  display:flex
  top:174px
  bottom:46px
  width:100%
  overflow:hidden
  .menu-wrapper
    flex:0 0 80px
    width:80px// 这个是为了在安卓系统下兼容性
    background:#f3f5f7
    .menu-item
      display: table
      height:54px
      width:56px
      padding:0 12px
      line-height:14px
      .icon
        display:inline-block
        width:12px
        height:12px
        margin-right:2px
        vertical-align:top
        background-size:12px 12px
        background-repeat:no-repeat
        &.decrease
          bg-img('decrease_3')
        &.discount
          bg-img('discount_3')
        &.guarantee
          bg-img('guarantee_3')
        &.invoice
          bg-img('invoice_3')
        &.special
          bg-img('special_3')
      .text
        display:table-cell
        width:56px
        vertical-align:middle
        font-size:12px
        border-1px(rgba(7,17,27,0.1))
  .stamps-wrapper
    flex:1//如果存在剩余空间,放大这个div
    .stamp-title
      padding-left:14px
      height:26px
      line-height:26px
      border-left:2px solid #d9dde1
      font-size:14px
      color:rgb(147,153,159)
      background:#f3f5f7
    .stamp-item
      display:flex
      margin:18px
      padding-bottom:18px
      border-1px(rgba(7,17,27,0.1))
      &:last-child
        border-none()
        margin-bottom:0
      .icon
        flex:0 0 57px
        margin-right:10px
      .stamp-content
        flex:1
        .stamp-name
          margin:2px 0 8px 0
          height:14px
          line-height:14px
          font-size:14px
          color:rgb(7,17,27)
        .stamp-desc,.stamp-extra
          line-height:10px
          font-size:10px
          color:rgb(147,153,159)
        .stamp-desc
          line-height:12px
          margin-bottom:8px
        .stamp-extra
          .stamp-count
            margin-right:12px
        .stamp-price
          font-weight:700
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
          position: absolute
          right: 0
          bottom: 12px




</style>