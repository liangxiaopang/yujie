<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<h1 class="title">{{seller.name}}</h1>
				<div class="desc">
					<star class="star" :size="36" :score="seller.score"></star>
					<span class="text">({{seller.ratingCount}})</span>
					<span class="text">月售{{seller.sellCount}}</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2>包邮</h2>
						<div class="content">
							满<span class="stress">{{seller.minPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>会员群</h2>
						<div class="content">
							<span class="stress">{{seller.membershipgroup}}</span>
						</div>
					</li>
					<li class="block">
						<h2>联系电话</h2>
						<div class="content">
							<span class="stress">{{seller.connect}}</span>
						</div>
					</li>
				</ul>
				<div class="favorite" @click='toggleFavorite'>
					<span class="icon-favorite" :class="{'active':favorite}"></span>
					<span class="text">{{favoriteText}}</span>
				</div>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<div class="content-wrapper border-1px">
					<p class="content">{{seller.bulletin}}</p>
				</div>
				<ul v-if="seller.supports" class="supports">
					<li class="support-item border-1px" v-for="(item,index) in seller.supports">
						<span class="icon" :class="classMap[seller.supports[index].type]"></span>
						<span class="text">{{seller.supports[index].description}}</span>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picWrapper">
				    <ul class="pic-list" ref='picList'>
				    	<li class="pic-item" v-for="pic in seller.pics">
				    		<img :src="pic" width="120" height="90">
				    	</li>
				    </ul>		
				</div>
			</div>
			<split></split>
			<div class="info">
				<h1 class="title border-1px">关于钰杰</h1>
				<ul>
					<li class="info-item" v-for="info in seller.infos">{{info}}</li>
				</ul>
			</div>
		</div>
	</div>	
</template>
<script type="text/ecmascript-6">
import split from '../split/split'
import star from '../star/star'
// import BScroll from 'better-scroll'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  },
  components: {
    split,
    star
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.seller
  position:absolute
  top:174px
  left:0
  bottom:0
  width:100%
  overflow:auto
  padding:18px
  .overview
    position:relative
    .title
      margin-bottom:8px
      line-height:14px
      font-size:14px
      color:rgb(7,17,27)
    .desc
      padding-bottom:18px
      border-1px(rgba(7,17,27,.2))
      font-size:0
      .star
        display:inline-block
        margin-right:8px
        vertical-align:middle
      .text
        display:inline-block
        font-size:10px
        margin-right:12px
        line-height:18px
        vertical-align:middle
        color:rgb(77,85,93)
    .remark
      display:flex
      padding:18px 0
      .block
        flex:1
        border-right:1px solid rgb(7,17,27,.2)
        text-align:center
        $:last-child
          border-none:none
        h2
          margin-bottom:4px
          line-height:10px
          font-size:10px
          color:rgb(147,153,159)
        .content
          line-height:20px
          font-size:10px
          color:rgb(7,17,27)
          .stress
            font-size:20px
    .favorite
      position:absolute
      width:50px
      right:11px
      top:18px
      text-align:center
      .icon-favorite
        display:block
        margin-bottom:4px
        line-height:24px
        font-size:24px
        color:#d4d6d9
        &.active
          color:rgb(240,20,20)
      .text
        line-height:10px
        font-size:10px
        color:rgb(77,85,93)
  .bulletin
    padding-top:18px
    .title
      margin-bottom:8px
      line-height:14px
      font-size:14px
      color:rgb(7,17,27)
    .content-wrapper
      padding:0 28px 16px 14px
      border-1px(rgba(7, 17, 27, .2))
      .content
        line-height:24px
        font-size:12px
        color:rgb(240,20,20)
    .supports
      .support-item
        padding:18px
        border-1px(rgba(7,17,27,.2))
        &:last-child
          border-none()
      .icon
        display:inline-block
        width:16px
        height:16px
        vertical-align:top
        margin-right:6px
        background-size:16px 16px
        background-repeat:no-repeat
        &.decrease
          bg-img('decrease_4')
        &.discount
          bg-img('discount_4')
        &.guarantee
          bg-img('guarantee_4')
        &.invoice
          bg-img('invoice_4')
        &.special
          bg-img('special_4')
      .text
        font-size:12px
        line-height:16px
        color:rgb(7,117,127)
  .pics
    padding:18px 0
    .title
      margin-bottom:12px
      line-height:14px
      font-size:14px
      clor:rgb(7,17,27)
    .pic-wrapper
      width:100%
      overflow:hidden
      white-space:nowrap
      .pic-list
        font-size:0
        .pic-item
          display:inline-block
          margin-right:6px
          width:120px
          height:90px
          &:last-child
            margin-right:0
  .info
    padding-top:18px
    color:rgb(7,17,27)
    .title
      padding-bottom:12px
      line-height:14px
      font-size:14px
      border-1px(rgba(7, 17, 27, 0.1))
    .info-item
      padding:16px 12px
      line-height:16px
      font-size:12px
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child
        border-none()





























</style>