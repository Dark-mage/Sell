<template>
 <div class="shopcar">
     <div class="content" @click="toggleList">
         <div class="content-left">
             <div class="logo-warpper">
                <div class="logo" :class="{'highlight':totalCount>0}">
                    <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
                </div>
                <div class="num" v-show="totalCount>0">{{totalCount}}</div>
             </div>
             <div class="price" :class="{'highlight':totalPrice>0}">¥{{totalPrice}}</div>
             <div class="desc">另需配送费¥{{deliveryPrice}}元</div>
         </div>
         <div class="content-right" @click.stop.prevent="pay">
             <div class="pay" :class="payClass">{{payTip}}</div>
         </div>
     </div>
     <div class="ball-container">
        <transition name="drop" v-for="(ball,index) in balls"  :key="index"
         @before-enter="beforeEnter"
         @enter="enter"
         @after-enter="afterEnter">
            <div  v-show="ball.show" class="ball">
                <div class="inner inner-hook"></div>
            </div>
        </transition>
     </div>
     <transition name="slide">
      <div class="shopcarlist-warpper" v-show="listShow">
            <div class="shopcarlist-header">
              <span class="title">购物车</span>
              <span class="clear" @click="cliearfoods">清空</span>
            </div>
            <div class="shopcarlist" ref="shopcarlist">
              <ul>
                <li v-for="(food,index) in selectFoods" :key="index" class="shopcarlist-item">
                  <div class="name">{{food.name}}</div>
                  <div class="price">¥{{food.price*food.count}}</div>
                  <div class="cartcontrol-warpper">
                    <cartControl :food="food"></cartControl>
                  </div>
                </li>
              </ul>
            </div>
          </div>
     </transition>
     <transition name="fade">
       <div class="listMask" @click="hideList" v-show="listShow" ></div>
     </transition>
 </div>
</template>

<script type='text/ecmascript-6'>
import cartControl from '../cartControl/cartControl'
import BScroll from 'better-scroll'
export default {
  data() {
    return {
      balls: [
        { show: false },
        { show: false },
        { show: false },
        { show: false },
        { show: false }
      ],
      dropBalls: [],
      fold: true
    }
  },
  props: {
    minPrice: {
      Number,
      default: 0
    },
    deliveryPrice: {
      Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default() {
        return [
          {
            price: 0,
            count: 0
          }
        ]
      }
    }
  },
  components: { cartControl },
  computed: {
    totalPrice() {
      let total = 0
      for (let i = 0; i < this.selectFoods.length; i++) {
        total += this.selectFoods[i].price * this.selectFoods[i].count
      }
      this.selectFoods.forEach(food => {
        total += food.price * food.count
      })
      return total
    },
    totalCount() {
      let count = 0
      this.selectFoods.forEach(food => {
        count += food.count
      })
      return count
    },
    payClass() {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      }
      return 'enough'
    },
    payTip() {
      if (this.totalPrice === 0) {
        return `¥${this.minPrice}起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差¥${diff}起送`
      }
      return '结算'
    },
    listShow() {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.shopcarlist, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  },
  methods: {
    drop(el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = el
          this.dropBalls.push(ball)
          return
        }
      }
    },
    beforeEnter(el) {
      let count = this.balls.length
      while (count--) {
        let ball = this.balls[count]
        if (ball.show) {
          // 获取小球的相对于视口的位移(小球高度)
          let rect = ball.el.getBoundingClientRect()
          let x = rect.left - 32
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0,${y}px,0)`
          el.style.transform = `translate3d(0,${y}px,0)`
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = `translate3d(${x}px,0,0)`
          inner.style.transform = `translate3d(${x}px,0,0)`
        }
      }
    },
    enter(el) {
      /* eslint-disable no-unused-vars */
      let rf = el.offsetHeight
      this.$nextTick(() => {
        // 触发重绘html
        el.style.webkitTransform = 'translate3d(0,0,0)'
        el.style.transform = 'translate3d(0,0,0)'
        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translate3d(0,0,0)'
        inner.style.transform = 'translate3d(0,0,0)'
      })
    },
    afterEnter(el) {
      let ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    },
    cliearfoods() {
      this.selectFoods.forEach(food => {
        food.count = 0
      })
    },
    toggleList() {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    pay() {
      if (this.totalPrice < this.minPrice) {
        return
      }
      window.alert('支付中')
    },
    hideList() {
      this.fold = true
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.shopcar
  position fixed
  left 0
  bottom 0
  z-index 50
  width 100%
  height 48px
  .content
    display flex
    background-color #141d27
    color rgba(255, 255, 255, 0.4)
    font-size 0
    .content-left
      flex 1
      .logo-warpper, .price, .desc
        display inline-block
        vertical-align top
      .logo-warpper
        position relative
        width 56px
        height 56px
        background-color #141d27
        border-radius 50%
        top -10px
        margin 0 12px
        box-sizing border-box
        padding 6px
        .logo
          width 100%
          height 100%
          border-radius 50%
          text-align center
          background #2b343c
          &.highlight
            background rgb(0, 120, 220)
          .icon-shopping_cart
            line-height 44px
            font-size 24px
            color #80858a
            &.highlight
              color #fff
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 12px
          border-radius 12px
          background-color rgb(240, 20, 20)
          box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
          font-size 9 px
          font-weight 700
          line-height 16px
          text-align center
      .price
        margin-top 12px
        padding-right 12px
        font-size 16px
        line-height 24px
        font-weight 700
        box-sizing border-box
        border-right 1px solid rgba(255, 255, 255, 0.1)
        &.highlight
          color #fff
      .desc
        font-size 14px
        line-height 24px
        margin 12px 0 0 12px
    .content-right
      width 105px
      .pay
        height 48px
        line-height 48px
        font-weight 700
        font-size 12px
        text-align center
        &.not-enough
          background #2b333b
        &.enough
          background #00b43c
          color #fff
  .ball-container
    .ball
      position fixed
      left 32px
      bottom 22px
      z-index 200
      transition all 0.6s cubic-bezier(0.49, -0.29, 0.75, 0.41)
      .inner
        width 16px
        height 16px
        border-radius 50%
        background rgb(0, 160, 220)
        transition all 0.4s linear
  .slide-enter-active, .slide-leave-active
    transition all 0.8s
  .slide-enter, .slide-leave-to
    opacity: 0
    transform: translate3d(0, 100%, 0)  
  .shopcarlist-warpper
    position absolute
    left 0
    bottom 48px
    max-height 306px
    overflow: hidden
    background-color #fff
    width 100%
    z-index -1
    .shopcarlist-header
      height 40px
      padding 0 18px
      background #f3f5f7
      font-size 0
      border-1px(rgba(7, 17, 27, 0.1))
      .title
        display inline-block
        width 50%
        font-size 14px
        color rgb(7, 17, 27)
        line-height 40px
      .clear
        display inline-block
        width 50%
        text-align right
        font-size 12px
        color rgb(0, 160, 220)
        line-height 40px
    .shopcarlist
      padding 0 18px
      .shopcarlist-item
        display flex
        position relative
        height 24px
        padding 12px 0
        line-height 24px
        font-size 0
        .name
          flex 1
          font-size 14px
          color rgb(7, 17, 27)
        .price
          margin 0 84px 0 18px
          font-size 14px
          font-weight 700px
          color rgb(240, 20, 20)
        .cartcontrol-warpper
          position absolute
          right 0
          bottom 0
  .listMask
    position :fixed
    width :100%
    height :100%
    background :rgba(7,17,27,0.6)
    left :0
    top:0
    z-index :-2
    backdrop-filter: blur(10px)
  .fade-enter-active, .fade-leave-active
    transition: all 0.6s
  .fade-enter, .fade-leave-to
    opacity :0
    background :rgba(7,17,27,0)           
</style>
