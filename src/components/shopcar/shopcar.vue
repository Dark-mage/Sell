<template>
 <div class="shopcar">
     <div class="content">
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
         <div class="content-right">
             <div class="pay" :class="payClass">{{payTip}}</div>
         </div>
     </div>
 </div>
</template>

<script type='text/ecmascript-6'>
 export default {
   data () {
     return {

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
               return [{
                   price: 0,
                   count: 0
               }]
           }
       }
   },
   components: {

   },
   computed: {
       totalPrice() {
           let total = 0
           for (let i = 0; i < this.selectFoods.length; i++) {
               total += this.selectFoods[i].price * this.selectFoods[i].count
           }
           this.selectFoods.forEach((food) => {
               total += food.price * food.count
           })
           return total
       },
       totalCount() {
           let count = 0
           this.selectFoods.forEach((food) => {
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
       }
   }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "../../common/stylus/mixin.styl"
    .shopcar
        position :fixed
        left: 0
        bottom: 0
        z-index 50
        width :100%
        height :48px
        .content
            display :flex
            background-color :#141d27
            color:rgba(255,255,255,0.4)
            font-size :0 
            .content-left
                flex :1
                .logo-warpper,.price,.desc
                    display :inline-block
                    vertical-align:top 
                .logo-warpper 
                    position :relative
                    width :56px
                    height :56px
                    background-color :#141d27
                    border-radius :50%
                    top : -10px
                    margin: 0 12px 
                    box-sizing :border-box
                    padding :6px
                    .logo
                        width:100%
                        height :100%
                        border-radius :50%
                        text-align :center
                        background: #2b343c
                        &.highlight
                            background: rgb(0,120,220)
                        .icon-shopping_cart
                            line-height: 44px
                            font-size: 24px
                            color: #80858a
                            &.highlight
                                color:#fff
                    .num
                        position:absolute
                        top: 0
                        right: 0
                        width: 24px
                        height: 12px
                        border-radius: 12px
                        background-color :rgb(240,20,20)
                        box-shadow : 0 4px 8px 0 rgba(0,0,0,0.4)
                        font-size :9 px
                        font-weight: 700
                        line-height: 16px
                        text-align :center 
                .price
                    margin-top:12px 
                    padding-right : 12px
                    font-size 16px
                    line-height :24px
                    font-weight :700
                    box-sizing: border-box
                    border-right : 1px solid rgba(255,255,255,0.1)
                    &.highlight
                        color:#fff
                .desc
                    font-size 14px
                    line-height :24px
                    margin: 12px 0 0 12px
            .content-right
                width: 105px
                .pay
                    height: 48px
                    line-height:48px 
                    font-weight :700 
                    font-size :12px
                    text-align :center
                    &.not-enough
                        background: #2b333b
                    &.enough
                        background: #00b43c
                        color: #fff
</style>
