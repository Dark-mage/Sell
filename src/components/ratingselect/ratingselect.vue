<template>
 <div class="ratingselect">
     <div class="rating-type border-1px">
         <span class="block positive" :class="{'active':2===selectedType}" @click="select(2,$event)">
             {{desc.all}}<span class="count">{{ratings.length}}</span>
         </span>
         <span class="block positive" :class="{'active':0===selectedType}" @click="select(0,$event)">
             {{desc.positive}}<span class="count">{{positive.length}}</span>
         </span>
         <span class="block negative" :class="{'active':1===selectedType}" @click="select(1,$event)">
             {{desc.negative}}<span class="count">{{negative.length}}</span>
         </span>
     </div>
     <div class="switch" @click="toggleContent">
         <span class="icon-check_circle" :class="{'active':onlyContented}"></span><span class="text">只看有内容的评价</span>
     </div>
 </div>
</template>

<script type='text/ecmascript-6'>
const ALL = 2
const POSITIVE = 0
const NEGATIVE = 1
export default {
  data() {
    return {
      selectedType: this.selectType,
      onlyContented: this.onlyContent
    }
  },
  props: {
    ratings: {
      type: Array,
      default() {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: true
    },
    desc: {
      type: Object,
      default() {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  computed: {
    positive() {
      return this.ratings.filter(rating => rating.rateType === POSITIVE)
    },
    negative() {
      return this.ratings.filter(rating => rating.rateType === NEGATIVE)
    }
  },
  methods: {
    select(type, event) {
      if (!event._constructed) {
        return
      }
      this.selectedType = type
      this.$emit('ratingtypeSelect', type)
    },
    toggleContent(event) {
      if (!event._constructed) {
        return
      }
      this.onlyContented = !this.onlyContented
      this.$emit('contentToggle', this.onlyContented)
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin.styl'
.ratingselect
    .rating-type
        margin :18px 0
        padding-bottom :18px
        font-size :0
        border-1px(rgba(7,17,27,0.1))
        .block
            display :inline-block
            margin-right 8px
            padding: 8px 12px
            font-size :12px
            color:rgb(77,85,93)
            line-height :16px
            &.active
                color:#fff
            &.positive
                background :rgba(0,160,220,0.2)
                &.active
                    background :rgb(0,160,220)
            &.negative
                background :rgba(77,85,93,0.2)
                &.active
                    background :rgb(77,85,93)              
            .count
                font-size :8px
    .switch
        margin-top :12px
        display :flex
        align-items :center
        .icon-check_circle
            color:rgb(147,153,159)
            font-size :24px
            margin-right :4px
            &.active
                color:#00c850
        .text
            color:rgb(147,153,159)             
            font-size :12px
            line-height :24px
</style>
