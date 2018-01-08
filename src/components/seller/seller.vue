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
    position: relative
    padding: 18px
    .name
      font-size :14px
      line-height :14px
      color:rgb(7,17,27)
      margin-bottom :8px
    .star-warpper
      display :flex
      align-items :center
      padding-bottom :18px
      font-size :10px
      border-1px(rgba(7,17,27,0.1))
      .ratingCount, .sellCount
        display :inline-block
        vertical-align :top
        line-height :18px
      .ratingCount
        color:rgb(77,85,93)
        margin : 0 12px 0 8px
    .info-warpper
      padding-top :18px
      display :flex
      .info-item
        flex :1
        text-align :center
        border-right :1px solid rgba(7,17,27,0.1)
        &:last-child
          border-right :none
        .item-title
          color:rgb(147,153,159)
          font-size :10px
          margin-bottom: 4px
        .item-text
          font-size :10px
          line-height: 24px
          color:rgb(7,17,27)
          .text
            font-size :24px

</style>
