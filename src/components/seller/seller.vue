<template>
 <div class="seller" ref="sellerWarpper">
   <div class="seller-content">
     <div class="seller-header">
       <h2 class="name">{{seller.name}}</h2>
       <div class="star-warpper">
         <star :size=36 :score="seller.score"></star>
         <span class="ratingCount">({{seller.ratingCount}})</span><span class="sellCount">月售{{seller.sellCount}}单</span>
       </div>
       <ul class="info-warpper">
           <li class="info-item">
             <p class="item-title">起送价</p>
             <p class="item-text"><span class="text">{{seller.minPrice}}</span>元</p>
           </li>
           <li class="info-item">
             <p class="item-title">商家配送</p>
             <p class="item-text"><span class="text">{{seller.deliveryPrice}}</span>元</p>
           </li>
           <li class="info-item">
             <p class="item-title">平均配送时间</p>
             <p class="item-text"><span class="text">{{seller.deliveryTime}}</span>分钟</p>
           </li>
        </ul>
     </div>
     <split></split>
     <div class="bulletin-warpper">
       <h3 class="title">公告与活动</h3>
       <div class="bulletin">{{seller.bulletin}}</div>
       <ul>
         <li v-for="(support,z) in seller.supports" :key="z" class="bulletin-item">
           <i class="icon" :class="claMap[support.type]" v-show="support.type>=0"></i>
           <span class="description">{{support.description}}</span>
         </li>
       </ul>
     </div>
     <split></split>
     <div class="carousel-warpper">
       <h3 class="title">商家实景</h3>
       <div class="carousel" ref="carousel">
         <ul class="carousel-content" v-show="seller.pics">
           <li class="carousel-item" v-for="(pic,index) in seller.pics" :key="index">
             <img :src="pic" alt="" width="120" height="90">
           </li>
         </ul>
       </div>
     </div>
     <split></split>
     <div class="infos-warpper">
       <h3 class="title">商家信息</h3>
       <ul>
         <li v-for="(info ,index) in seller.infos" :key="index" class="info-item">{{info}}</li>
       </ul>
     </div>
   </div>
 </div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star'
import split from '../split/split'
import BScroll from 'better-scroll'
export default {
  data() {
    return {}
  },
  props: {
    seller: {
      Object
    }
  },
  created() {
    this.claMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    this.$nextTick(() => {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.sellerWarpper, {
          click: true
        })
        // this.scrollX = new BScroll(this.$refs.carousel, {
        //   click: true,
        //   scrollX: true,
        //   scrollY: false,
        //   snap: {
        //     loop: this.loop,
        //     threshold: 0.3,
        //     speed: 400
        //   }
        // })
      } else {
        this.scroll.refresh()
        // this.scrollX.refresh()
      }
    })
  },
  components: {
    star,
    split,
    BScroll
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.seller-header
  position relative
  padding 18px
  .name
    font-size 14px
    line-height 14px
    color rgb(7, 17, 27)
    margin-bottom 8px
  .star-warpper
    display flex
    align-items center
    padding-bottom 18px
    font-size 10px
    border-1px(rgba(7, 17, 27, 0.1))
    .ratingCount
      color rgb(77, 85, 93)
      margin 0 12px 0 8px
  .info-warpper
    padding-top 18px
    display flex
    .info-item
      flex 1
      text-align center
      border-right 1px solid rgba(7, 17, 27, 0.1)
      &:last-child
        border-right none
      .item-title
        color rgb(147, 153, 159)
        font-size 10px
        margin-bottom 4px
      .item-text
        font-size 10px
        line-height 24px
        color rgb(7, 17, 27)
        .text
          font-size 24px
.bulletin-warpper
  padding 18px 18px 0 18px
  .title
    color rgb(7, 17, 27)
    font-size 14px
    margin-bottom 8px
  .bulletin
    padding 0 12px 16px 12px
    font-size 12px
    line-height 24px
    color rgb(240, 20, 20)
  .bulletin-item
    padding 16px 12px
    border-1px(rgba(7, 17, 27, 0.1))
    line-height 16px
    font-size 0
    .icon
      display inline-block
      width 16px
      height 16px
      background-size 16px 16px
      background-repeat no
      margin-right 6
      &.decrease
        bg-image('decrease_2')
      &.discount
        bg-image('discount_2')
      &.guarantee
        bg-image('guarantee_2')
      &.invoice
        bg-image('invoice_2')
      &.special
        bg-image('special_2')
    .description
      display inline-block
      vertical-align top
      color rgb(7, 17, 27)
      font-size 12px
.carousel-warpper
  padding 18px 0 18px 18px
  .title
    margin-bottom 12px
  .carousel
    width 100%
    overflow hidden
    ul
      display flex
      .carousel-item
        margin-right 6px
.infos-warpper
  padding 18px 18px 0 18px
  .title
    margin-bottom 12px
  .info-item
    padding 16px 12px
    border-1px(rgba(7, 17, 27, 0.1))
    line-height 16px
    font-size 12px
</style>
