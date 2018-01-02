<template>
 <div class="cart-control">
     <transition name="move">
        <div class="cart-decrease" @click="decreaseCount($event)" 
        v-show="food.count>0">
            <i class="icon-remove_circle_outline inner"></i>
        </div>
     </transition>
     <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
     <div class="cart-add icon-add_circle" @click="addCount($event)"></div>
 </div>
</template>

<script type='text/ecmascript-6'>
import Vue from 'vue'
 export default {
   data () {
     return {

     }
   },
   props: {
       food: {
           type: Object
       }
   },
   methods: {
       addCount(event) {
           if (!event._constructed) {
               return
           }
           if (!this.food.count) {
               Vue.set(this.food, 'count', 1)
           } else {
               this.food.count++
           }
       },
       decreaseCount(event) {
           if (!event._constructed) {
               return
           }
           this.food.count--
       }
   },
   components: {

   }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
    .cart-control
        font-size :0
        .cart-decrease
            display :inline-block
            opacity : 1
            .inner 
                display :inline-block
                font-size :24px
                color:rgb(0,160,220)
                line-height : 24px
                transition: all 0.8s ease
                transform : rotate(0)
        .move-enter-active, .move-leave-active
            transition: all 0.8s ease
        .move-enter,.move-leave-to
            opacity :0
            transform : translate3d(24px, 0, 0)
            .inner
                transform: rotate(180deg)
        .cart-count
            display :inline-block
            line-height :24px
            font-size :10px
            width :24px
            text-align :center
            vertical-align :top
            color:rgb(147,153,159)
        .cart-add
            display :inline-block
            font-size :24px
            color:rgb(0,160,220)    
 
</style>
