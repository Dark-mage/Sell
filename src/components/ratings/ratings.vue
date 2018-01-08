<template>
 <div class="ratings-warpper" ref="ratingsWarpper">
   <div class="bs-content">
     <div class="ratings-header">
       <div class="header-left">
         <h4 class="score">{{seller.score}}</h4>
         <span>综合评分</span>
         <p class="rankRate">高于周边商家{{seller.rankRate}}%</p>
       </div>
       <div class="header-right">
         <div class="score-content">
           <span class="text">服务态度</span>
           <star :size=36 :score="seller.serviceScore"></star>
           <span class="score_text">{{seller.serviceScore}}</span>
         </div>
         <div class="score-content">
           <span class="text">商品评分</span>
           <star :size=36 :score="seller.foodScore"></star>
           <span class="score_text">{{seller.foodScore}}</span>
         </div>
         <div class="score-content">
           <span class="text">送达时间</span>
           <span class="deliveryTime">{{seller.deliveryTime}}分钟</span>
         </div>
       </div>
     </div>
     <split></split>
     <div class="ratingselect-warpper border-1px">
        <ratingselect :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent" 
        @contentToggle="contentToggle" @ratingtypeSelect="ratingtypeSelect"></ratingselect>
     </div>
     <div class="ratings-warpper">
       <ul v-show="ratings && ratings.length">
         <li v-for="(rating,index) in ratings" :key="index" class="rating-item" v-show="showRating(rating.rateType,rating.text)">
           <div class="avatar">
             <img :src="rating.avatar" alt="" width="28" height="28">
           </div>
           <div class="user-warppper">
             <span class="username">{{rating.username}}</span><span class="rateTime">{{rating.rateTime | formatDate}}</span>
           </div>
           <div class="rating-star-warpper">
             <star :size=24 :score="rating.score"></star>
             <span class="deliveryTime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
           </div>
           <p class="text">{{rating.deliveryTime}}</p>
           <div class="recommend-warpper">
             <i :class="{'icon-thumb_up':0===rating.rateType,'icon-thumb_down':1===rating.rateType}"></i>
             <span v-for="(rec,i) in rating.recommend" :key="i" class="recommend">{{rec}}</span>
           </div>
         </li>
       </ul>
       <div class="no-rating" v-show="!ratings || !ratings.length">暂无评价</div>
     </div>
   </div>
 </div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star'
import split from '../split/split'
import BScroll from 'better-scroll'
import { formatDate } from '../../common/js/date'
import ratingselect from '../ratingselect/ratingselect'
const ERR_NO = 0
const ALL = 2
export default {
  data() {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: true
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  methods: {
    contentToggle(onlyContent) {
      this.onlyContent = onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    ratingtypeSelect(type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    showRating(rateType, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      }
      return rateType === this.selectType
    }
  },
  filters: {
    formatDate(time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  created() {
    this.$http.get('/api/ratings').then(res => {
      res = res.body
      if (res.errno === ERR_NO) {
        this.ratings = res.data
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.ratingsWarpper, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      }
    })
  },
  components: {
    star,
    split,
    ratingselect
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  @import '../../common/stylus/mixin.styl'
  .ratings-header
    display :flex
    padding :18px 0
    .header-left
      flex:1
      text-align :center
      font-size :12px
      color:rgb(7,17,27)  
      .score
        font-size :24px
        color:rgb(255,153,0)
        line-height :28px
        margin-bottom :6px
      .rankRate
        margin-top :8px
        padding-bottom :6px
        line-height :10px
        color:rgb(147,153,159)
    .header-right
      width :237px
      box-sizing :border-box
      padding:0 24px
      .score-content
        line-height :18px
        display :flex
        align-content :center
        .text
          font-size :12px
          color:rgb(7,17,27)
          margin-bottom :8px
          margin-right :12px
          &:last-children
            margin-bottom :0
        .score_text
            color:rgb(255,153,0)
            font-size :12px
            margin-left :12px     
        .deliveryTime
          color:rgb(147,153,159)
          font-size :12px
  .ratingselect-warpper
    padding: 0 18px 18px 18px
    border-1px(rgb(147,153,159))
  .ratings-warpper
    padding: 0 18px  
    .rating-item
      position :relative    
      font-size :10px
      color:rgb(147,153,159)
      padding: 18px 0 18px 48px
      border-1px(rgba(7,17,27,0.1))
      .avatar
        position :absolute
        left :0px
        top:18px
        width :28px
        height :28px
        border-radius :50%
        overflow: hidden
      .user-warppper
        margin-bottom :4px
        .username , .rateTime
          display :inline-block
          line-height :12px
          width :50%
        .username
          color:rgb(7,17,27)
        .rateTime
          text-align :right
      .rating-star-warpper
          display:flex
          align-items :center
          margin-bottom :6px
          .deliveryTime
            margin-left :6px
            line-height :12px
      .text
        color:rgb(7,17,27)
        font-size :12px
        line-height :18px
        margin-bottom 8px
      .recommend-warpper
      margin-bottom :18px
      &:last-child
        margin-bottom :0
      .icon-thumb_up,.icon-thumb_down
        font-size :12px
        line-height :16px
      .icon-thumb_up
        color:rgb(0,160,220)  
      .icon-thumb_down
        color:rgb(183,187,191)
      .recommend
        display :inline-block        
        margin-left :8px
        padding : 0 6px
        border:1px solid rgba(7,17,27,0.1)
        border-radius :2px
        background-color :#fff
        color:rgb(147,153,159)
        line-height :16px
        font-size :9px
        margin-bottom: 8px 
    .no-rating
      padding 16px 0
      font-size 12px
      color rgb(147, 153, 159)      
</style>
