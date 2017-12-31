<template>
 <div class="goods">
   <div class="menu-warpper">
      <ul>
        <li v-for="(menu,index) in goods" :key='index' class="menu-item">
          <span class="text border-1px" >
            <span v-show="menu.type>=0" class="icon" :class="claMap[menu.type]"></span>{{menu.name}}
          </span>
        </li>
      </ul>
   </div>
   <div class="foods-warpper">
     <ul>
       <li v-for="(menu,index) in goods" :key='index' class="food-list">
          <h2 class="title">{{menu.name}}</h2>
          <ul>
            <li v-for="(food,i) in menu.foods" :key='i' class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" alt="" width="57" height="57">
              </div>
              <div class="content">
                <h3 class="name">{{food.name}}</h3>
                <p class="description">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="now">¥{{food.price}}</span><span v-show="food.oldPrice" class="oldPrice">¥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
       </li>
     </ul>
   </div>
 </div>
</template>

<script type='text/ecmascript-6'>
const ERR_NO = 0
 export default {
   data () {
     return {
       goods: {}
     }
   },
   components: {

   },
   created () {
     this.claMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
     this.$http.get('/api/goods').then((res) => {
        res = res.body
        if (res.errno === ERR_NO) {
          this.goods = res.data
      }
     })
   }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
  .goods
    display : flex
    position: absolute
    top: 176px
    bottom:46px
    width: 100%
    overflow : hidden
    .menu-warpper
      width :80px
      .menu-item
        padding: 0 12px
        height :54px
        display:flex
        justify-content :center
        align-items :center
        background :#f3f5f7
        font-size :0
        border-1px(rgba(7,17,27,0.1))
        &:after
          width :70%
          left :15%
        .icon
          display :inline-block
          width:12px
          height :12px
          background-size : 12px 12px
          background-repeat :no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          font-size :12px
          line-height :14px
    .foods-warpper
      flex :1
      .title
        height: 26px
        line-height: 26px    
        background-color :#f3f5f7
        color:rgb(147,153,159)
        font-size:12px
        text-indent : 14px
        border-left: 2px solid #d9dde1
      .food-item
        display : flex
        margin :18px
        padding-bottom :18px
        border-1px(rgba(7, 17, 27, 0.1))
        &:last-child
          border-none()
          margin-bottom: 0
        .icon
          width :57px 
          margin-right :10px
        .content
          flex: 1
          .name
            margin:2px 0 8px 0
            line-height :14px
            font-size :14px
            color:rgb(7,17,27)
          .description,.extra
            font-size :10px
            color:rgb(147,153,159)
            line-height :10px
          .description
            margin-bottom :8px
          .extra  
            .sellCount
              margin-right :12px
          .price
            line-height :24px
            font-weight 700      
            .now
              margin-right: 8px 
              font-size :14px
              color:rgb(240,20,20)
            .oldPrice
              text-decoration: line-through
              font-size :10px
              color:rgb(147,153,159)
</style>
