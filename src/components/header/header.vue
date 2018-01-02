<template>
 <div class="header">
     <div class="content-warpper">
        <div class="avatar">
          <img :src="seller.avatar" alt="" width="64" height="64">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}} / {{seller.deliveryTime}}分送达
          </div>
          <div v-if="seller.supports" class="supports">
            <span class="icon" :class="claMap[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
          <div v-if="seller.supports" class="support-count" @click="showDetail()">
            <span class="count">{{seller.supports.length}}个</span>
            <i class="icon-keyboard_arrow_right"></i>
          </div>
        </div>
     </div>
     <div class="bulletin-warpper" @click="showDetail()">
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
     </div>
     <div class="bankground">
        <img :src="seller.avatar" alt="" width="100%" height="100%">
     </div>
     <transition name="fade">
       <div v-show="detailShow" class="detail">
        <div class="detail-warpper clearfix">
          <div class="detail-main">
              <h1 class="name">{{seller.name}}</h1>
              <div class="star-warpper">
                <star :size=48 :score="seller.score"></star>
              </div>
              <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
              </div>
              <ul v-if="seller.supports" class="supports">
                <li class="support-item" v-for="supprot in seller.supports" v-bind:key="supprot.type">
                    <span class="icon" :class="claMap[supprot.type]"></span>
                    <span class="text">{{supprot.description}}</span>
                </li>
              </ul>
              <div class="title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
              </div>
              <div class="bulletin">
                <p class="content">{{seller.bulletin}}</p>
              </div>
          </div>
        </div> 
        <div class="detail-close" @click="showDetail()">
          <i class="icon-close"></i>
        </div>
     </div>
    </transition>
 </div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.claMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = !this.detailShow
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
    position: relative 
    color: #fff
    background-color :rgba(7,17,27,0.5)
    overflow: hidden
    .content-warpper
      position: relative
      display: flex
      padding: 24px 12px 18px 24px
      font-size: 0px
      .avatar
        display: inline-block
        vertical-align: top
        img 
          border-radius 2px
      .content
        margin-left: 16px 
        display: inline-block 
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top 
            width: 30px
            height: 18px  
            bg-image('brand')     
            background-size: 30px 18px
            background-repeat: no-repeat  
          .name
            margin-left: 6px
            font-weight: bold
            font-size: 16px   
            line-height: 18px
        .description
            font-size : 12px 
            line-height: 12px
            margin-bottom: 10px 
        .supports
            .icon
              display: inline-block
              width: 12px
              height: 12px
              vertical-align: top
              background-size: 12px 12px
              background-repeat: no-repeat
              margin-right: 4px                               
              &.decrease
                bg-image('decrease_1')
              &.discount
                bg-image('discount_1')
              &.guarantee
                bg-image('guarantee_1')
              &.invoice
                bg-image('invoice_1')
              &.special
                bg-image('special_1')
            .text
              line-height: 12px     
              font-size: 10px
        .support-count
          position: absolute
          right: 12px
          bottom: 14px
          padding: 0 8px
          line-height: 24px
          height : 24px
          border-radius: 14px
          background-color: rgba(0,0,0,0.2)   
          text-align: center
          .count
            font-size: 10px  
            margin-right: 2px
            display:inline-block 
            vertical-align: top  
          .icon-keyboard_arrow_right:before
            font-size: 10px
            line-height: 24px
    .bulletin-warpper
      position: relative 
      height:28px
      line-height: 28px
      padding: 0 22px 0 12px
      background-color:rgba(7,17,27,0.2)
      overflow:hidden
      text-overflow: ellipsis
      white-space: nowrap              
      .bulletin-title
        display:inline-block
        width :22px
        height :12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
        margin-top:7px
        vertical-align:top 
      .bulletin-text
        margin: 0 4px 
        font-size:10px
      .icon-keyboard_arrow_right
        position : absolute
        right: 12px 
        font-size:10px 
        top:9px 
    .bankground
      position: absolute
      left: 0
      top:0
      width:100%
      height:100%
      filter:blur(10px)
      z-index: -1
    .detail
      position : fixed
      left : 0
      top: 0
      width :100%
      height : 100%
      z-index : 100
      overflow :auto
      background:rgba(7,17,27,0.8)
      backdrop-filter: blur(10px)
      &.fade-enter-active, &.fade-leave-active
        transition : all 0.5s
      &.fade-enter, &.fade-level-to
        opacity : 0
        background:rgba(7,17,27,0)
      .detail-warpper
        width:100% 
        min-height :100%
        .detail-main
          margin-top:64px 
          padding-bottom : 64px
          .name
            font-size: 16px
            line-height :16px
            text-align :center
            font-weight: 700
          .star-warpper
            margin-top 16px
            text-align :center
          .title
            display: flex
            width :80%
            margin: 28px auto 24px auto
            .line
              flex :1
              position: relative
              top: -6px
              border-bottom: 1px solid rgba(255,255,255,0.2)
            .text
              padding : 0 12px
              font-size: 14px
              font-weight : 700
          .supports
            width :80%               
            margin :24px auto 0 auto
            .support-item
              padding: 0 12px
              margin-bottom :12px
              font-size: 0
              &:last-child
                margin-bottom :0
              .icon
                display :inline-block
                width :16px
                height :16px
                vertical-align :top
                background-size :16px 16px
                background-repeat :no
                margin-right :6px
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
              .text
                font-size :12px
                line-height :16px
          .bulletin
              width :80%               
              margin :0 auto    
              .content
                padding : 12px 0
                font-size :12px
                line-height :24px
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin:-64px auto 0 auto
        font-size :32px
        color:rgba(255,255,255,0.5)
        clear: both
</style>
