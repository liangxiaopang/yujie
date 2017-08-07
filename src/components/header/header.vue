  <template>
	<div class='header'>
	  <div class='content-wrap'>
	    <div class='avatar'>
	      <img width='64' height='64' src='./logo.png'>
	    </div>
	    <div class='content'>
	      <div class='title'>
	      	<span class='brand'></span>
	      	<span class="name">{{seller.name}}</span>
	      </div>
	      <div class='description'>
	      	{{seller.description}}/{{seller.deliveryTime}}小时送达
	      </div>
	      <div v-if='seller.supports' class='support'>
	      	<span class="icon" :class="classMap[seller.supports[0].type]"></span>
	      	<span class="text">{{seller.supports[0].description}}</span>
	      </div>
	    </div>
	    <div v-if='seller.supports' class='support-count' @click='detailshow'>
	    	<span class="count">{{seller.supports.length}}个</span>
	    	<i class='icon-keyboard_arrow_right'></i>
	    </div>
	  </div>
	<div class='bulletin-wrap' @click='detailshow'>
		<span class="bulletin-title"></span>
		<span class="bulletin-text">{{seller.bulletin}}</span>
		<i class='icon-keyboard_arrow_right'></i>
	</div>
	<div class='background'>
		<img :src="seller.avatar" width='100%' height="100%">
	</div>
	<transition name="fade">
		<div v-show='detailShow' class="detail">
		<div class='detail-wrap clearfix'>
			<div class="detail-main">
			    <h1 class='seller-name'>{{seller.name}}</h1>
			    <div class="star-wrapper">
			    	<star :size='48' :score='seller.score'></star>
			    </div>
			    <div class="line-title">
			    	<div class="line-title-line"></div>
			    	<div class="line-title-text">优惠信息</div>
			    	<div class="line-title-line"></div>
			    </div>
			    <ul v-if='seller.supports' class="supports">
			    	<li v-for='(item,index) in seller.supports' class='support-item'>
			    		<span class="icon" :class='classMap[index]'></span>
			    		<span class='text'>{{item.description}}</span>
			    	</li>
			    </ul>
			    <div class="line-title">
			    	<div class="line-title-line"></div>
			    	<div class="line-title-text">商家公告</div>
			    	<div class="line-title-line"></div>
			    </div>
			    <div class="bulletin">
			    	<p class="content">
			    		{{seller.bulletin}}
			    	</p>
			    </div>
			</div>
		</div>
		<div class='detail-close' @click='hideDetail'>
			<i class="icon-close"></i>
		</div>
	
	</div>
	</transition>
	</div>
</template>

<script type='text/ecmascript-6'>
  import star from '../star/star'

  export default {
    data () {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        detailShow: false
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    methods: {
      detailshow () {
        this.detailShow = true
      },
      hideDetail () {
        this.detailShow = false
      }
    },
    components: {
      star
    }
  }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.header
  color:#fff
  overflow:hidden
  background:rgba(7,17,27,0.5)
  position:relative
  .content-wrap
    padding:24px 12px 18px 24px
    font-size:0px
    position:relative
    .avatar
      display:inline-block
      vertical-align:top
      img
        border-radius:2px
    .content
      display:inline-block
      margin-left:16px
      .title
        margin:2px 0 8px 0
        .brand
          width:30px
          height:18px
          vertical-align:top
          display:inline-block
          bg-img('./brand')
          background-size:30px 18px
          background-repeat:no-repeat
        .name
          margin-left:6px
          font-size:16px
          line-height:18px
          font-weight:bold
      .description
        margin-bottom:10px
        line-height:12px
        font-size:12px
      .support
        .icon
          display:inline-block
          width:12px
          height:12px
          margin-right:4px
          vertical-align:top
          background-size:12px 12px
          background-repeat:no-repeat
          &.decrease
            bg-img('decrease_1')
          &.discount
            bg-img('discount_1')
          &.guarantee
            bg-img('guarantee_1')
          &.invoice
            bg-img('invoice_1')
          &.special
            bg-img('special_1')
        .text
          display:inline-block
          font-size:10px
          line-height:12px
    .support-count
      position:absolute
      right:12px
      bottom:16px
      padding:0 8px
      height:24px
      line-height:24px
      border-radius:14px
      background-color:rgba(0,0,0,0.2)
      text-align:center
      .count
        font-size:10px
        vertical-align:top
      .icon-keyboard_arrow_right
        margin-left:2px
        font-size:10px
        line-height:24px
  .bulletin-wrap
    height:28px
    position:relative
    line-height:28px
    padding:0 22px 0 12px
    background:rgba(7,17,27,0.2)
    white-space:nowrap
    overflow:hidden
    text-overflow:ellipsis
    .bulletin-title
      display:inline-block
      vertical-align:top
      margin-top:8px
      width:22px
      height:12px
      bg-img('bulletin')
      background-size: 22px 12px
      background-repeat:no-repeat
    .bulletin-text
      vertical-align:top
      font-size:10px
      margin:0 4px
    .icon-keyboard_arrow_right
      position:absolute
      right:12px
      top:4px
      margin-left:6px
      font-size:14px
      line-height:24px
  .background
    position:absolute
    top:0
    left:0
    width:100%
    height:100%
    z-index:-1
    filter:blur(10px)
  .fade-enter-active, .fade-leave-active
    transition: opacity 1s
  .fade-enter, .fade-leave-to
    opacity: 0
  .detail
    position:fixed
    z-index:50
    width:100%
    height:100%
    overflow:auto
    background:rgba(7,17,27,0.8)
    top:0
    left:0
    .detail-wrap
      min-height:100%
      width:100%
      display:inline-block
      .detail-main
        margin-top:64px
        padding-bottom:64px
        .seller-name
          line-height:16px
          font-size:16px
          text-align:center
          font-weight:700
        .star-wrapper
          margin-top:18px
          padding:2px 0
          text-align:center
        .line-title
          display:flex
          width:80%
          margin:28px auto 24px auto
          .line-title-line
            flex:1
            position:relative
            top:-6px
            border-bottom:2px solid rgba(255,255,255,0.2)
          .line-title-text
            padding:0 12px
            font-weight:700
            font-size:14px
        .supports
          width:80%
          margin:0px auto
          list-style:none
          .support-item
            padding:0 12px
            margin-bottom:12px
            font-size:0
            &:last-child
              margin-bottom:0
            .icon
              display:inline-block
              width:16px
              height:16px
              vertical-align:top
              margin-right:6px
              background-size:16px 16px
              background-repeat:no-repeat
              &.decrease
                bg-img('decrease_2')
              &.discount
                bg-img('discount_2')
              &.guarantee
                bg-img('guarantee_2')
              &.invoice
                bg-img('invoice_2')
              &.special
                bg-img('special_2')
            .text
              display:inline-block
              font-size:12px
              line-height:16px
        .bulletin
          width:80%
          margin:0 auto
          .content
            padding:0 12px
            line-height:24px
            font-size:12px
            font-weight:600
    .detail-close
      position:relative
      width:32px
      height:32px
      margin:-96px auto 0  auto
      clear:both
      font-size:32px
</style>