<template>
<transition name="fade">
 <div class="food" v-show="showFlag" ref="food">
    <div>
        <div class="food-header">
            <div class="food-img">
                <img :src="food.image" alt="">
            </div>
            <div class="back" @click="hideFood">
                <i class="icon-arrow_lift"></i>
            </div>
        </div>
        <div class="content">
            <h2 class="name">{{food.name}}</h2>
            <div class="desc">
                <span class="sellCount">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}</span>
            </div>
            <div class="price">
                ¥<span class="now">{{food.price}}</span><span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
            </div>
            <div v-show="food.count&&food.count>0" class="cartControl-warpper">
                <cartControl :food="food"></cartControl>
            </div>
            <transition name="cartfade">
                <div class="cartControl-warpper" v-show="!(food.count&&food.count>0)">
                    <div class="addNew" @click.stop.prevent="addNew">加入购物车</div>
                </div>
            </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info-warpper" v-show="food.info">
            <h4>商品介绍</h4>
            <p>{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
            <h4>商品评价</h4>
            <ratingselect :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent" 
            :desc="desc" @contentToggle="contentToggle" @ratingtypeSelect="ratingtypeSelect"></ratingselect>
        </div>
         <div class="rating-warpper">
                <ul v-show="food.ratings && food.ratings.length">
                    <li class="rating-item" v-for="(rating,j) in food.ratings" :key="j" v-show="showRating(rating.rateType,rating.text)">
                        <div class="user">
                            <span class="name">{{rating.username}}</span>
                            <img class="avatar" :src="rating.avatar" alt="" width="12" height="12">
                        </div>
                        <div class="time">{{rating.rateTime}}</div>
                        <div class="rating-content">
                            <i :class="{'icon-thumb_up':0===rating.rateType,'icon-thumb_down':1===rating.rateType}"></i>
                            <span class="text" v-show="rating.text">{{rating.text}}</span>
                        </div>
                    </li>
                </ul>
                <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
                    暂无评价
                </div>
            </div>
     </div>
 </div>
</transition>
</template>

<script type='text/ecmascript-6'>
import Vue from 'vue'
import BScroll from 'better-scroll'
import cartControl from '../cartControl/cartControl'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
const ALL = 2
export default {
  data() {
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
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    show() {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = true
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
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
    },
    hideFood() {
      this.showFlag = false
    },
    addNew(event) {
      this.$emit('cartAdd', event.target)
      Vue.set(this.food, 'count', 1)
    },
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
    }
  },
  components: { cartControl, split, ratingselect }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.fade-enter-active, .fade-leave-active
    transition all 0.2s
.fade-enter, .fade-leave-to
    opacity 0
    transform translate3d(100%, 0, 0)
.food
    position fixed
    left 0
    top 0
    bottom 48px
    width 100%
    background #fff
    z-index 30
    overflow hidden
    .food-header
        position relative
        .food-img
            width 100%
            height 0
            padding-top 100%
            img
                position absolute
                left 0
                top 0
                width 100%
                height 100%
        .back
            position absolute
            left 18px
            top 18px
            padding 5px
            background rgba(0, 0, 0, 0.2)
            .icon-arrow_lift
                font-size 14px
                color #fff
    .content
        position relative
        padding 18px
        .name
            font-size 14px
            font-weight 700
            color rgb(7, 17, 27)
            margin-bottom 8px
        .desc
            color rgb(147, 153, 159)
            font-size 10px
            margin-bottom 18px
            .sellCount
                margin-right 10px
        .price
            font-size 10px
            color rgb(240, 20, 20)
            .now
                font-size 14px
                font-weight 700
                margin-right 12px
            .old
                font-size 10px
                font-weight 700
                color rgb(147, 153, 159)
        .cartControl-warpper
            position absolute
            right 18px
            bottom 18px
            .addNew
                font-size 10px
                height 24px
                line-height 24px
                width 74px
                text-align center
                border-radius 12px
                color #fff
                background rgb(0, 160, 220)
        .cartfade-enter-active, .cartfade-leave-active
            transition all 0.2s
        .cartfade-enter, .cartfade-leave-to
            opacity 0
    .info-warpper
        padding 18px
        h4
            font-size 14px
            color rgb(7, 17, 27)
            margin-bottom 12px
        p
            font-size 12px
            color rgb(77, 85, 93)
            padding 0 8px
            line-height 24px
    .rating
        padding 18px 18px 12px 18px
        border-1px(rgba(7, 17, 27, 0.1))
        h4
            font-size 14px
            color rgb(7, 17, 27)
    .rating-warpper
        padding 18px
        .rating-item
            position relative
            padding 16px 0
            border-1px(rgba(7, 17, 27, 0.1))
            .user
                position absolute
                right 18px
                top 16px
                height 12px
                line-height 12px
                .name
                    display inline-block
                    vertical-align top
                    font-size 10px
                    color rgb(147, 153, 159)
                    margin-right 6px
            .time
                font-size 10px
                color rgb(147, 153, 159)
                line-height 12px
                margin-bottom 6px
            .rating-content
                font-size 12px
                line-height 16px
                color rgb(7, 17, 27)
                .icon-thumb_up
                    color rgb(0, 160, 220)
                .icon-thumb_down
                    color rgb(147, 153, 159)
        .no-rating
            padding 16px 0
            font-size 12px
            color rgb(147, 153, 159)
</style>
