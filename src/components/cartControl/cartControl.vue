<template>
 <div class="cart-control">
     <transition name="move">
        <div class="cart-decrease icon-remove_circle_outline" @click="decreaseCount($event)" 
        v-show="food.count>0"></div>
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
        .cart-decrease,.cart-add
            display :inline-block
            line-height :24px
            font-size :24px
            color:rgb(0,120,220)
        .cart-decrease
            &.move-enter-active,&.move-leave-active
                transition : all 0.5s
            &.move-enter,&.move-level-to
                opacity : 0
                transform: translate3d(24, 0, 0)
                transform: rotate(0)
            &.move-enter-to,&.move-level
                transform: translate3d(0, 0, 0)
                opacity : 1 
                transform: rotate(180)   
        .cart-count
            display :inline-block
            line-height :24px
            font-size :10px
            vertical-align :top
            width : 24px
            text-align :center    
            color:rgb(147,153,159)
 
</style>
