<template lang="html">
   <div class="goods">
   	 <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item, index) in goods" class="menu-item" @click="menuClick(index, $event)" :class="index==menuCurrentIndex?'menu-item-selected':'menu-item'">
            <span class="text border-1px">
              <span v-show="item.type>0" class="icon" :class="iconClassMap[item.type]"></span>
              {{item.name}}
            </span>
          </li>
        </ul>
   	 </div>

     <div class="foods-wrapper" ref="foodsWrapper">
       <ul>
         <li v-for="item in goods" class="food-list food-list-hook">
           <h1 class="title">{{item.name}}</h1>
           <ul>
             <li v-for="food in item.foods" class="food-item border-1px">
               <div class="icon">
                 <img :src="food.icon" alt="" width="57" height="57">
               </div>
               <div class="content">
                 <h2>{{food.name}}</h2>
                 <p class="description">{{food.description}}</p>
                 <div class="sell-info">
                   <span class="sellCount">月售{{food.sellCount}}份</span>
                   <span class="rating">好评率{{food.rating}}</span>
                 </div>
                 <div class="price">
                   <span class="newPrice">￥{{food.price}}</span>
                   <span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
                 </div>
               </div>
             </li>
           </ul>
         </li>
       </ul>
     </div>
   </div>
</template>

<script>
import BScroll from 'better-scroll'
const ERR_OK = 0
export default {
  // props: {
  //   seller: {
  //     type: Object
  //   }
  // },
  data() {
    return {
      goods: [],
      listHeight: [],
      foodsScrollY: 0
    }
  },
  computed: {
    menuCurrentIndex() {
      for (let i = 0, l = this.listHeight.length; i < l; i++) {
        let topHeight = this.listHeight[i]
        let bottomHeight = this.listHeight[i + 1]
        if (!bottomHeight || (this.foodsScrollY >= topHeight && this.foodsScrollY < bottomHeight)) {
          return i
        }
      }
      return 0
    }
  },
  created() {
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    })
  },
  methods: {
    _initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', (pos) => {
        this.foodsScrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight() {
      let foodList = this.$refs.foodsWrapper.querySelectorAll('.food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0, l = foodList.length; i < l; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.listHeight.push(height)
      }
    },
    menuClick(index, event) {
      if (!event._constructed) {
        return
      }
      this.foodsScroll.scrollTo(0, -this.listHeight[index], 300)
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin'
.goods
  display flex
  position absolute
  top 174px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 80px
    width 80px
    background #f3f5f7
    margin-top: 2px;
    .menu-item-selected
      background white
      font-weight 700
      margin-top -1px
    .menu-item,.menu-item-selected
      position relative
      display table
      height 54px
      line-height 14px
      width 56px
      padding 0 12px
      &:last-child:after
        content none
    .menu-item:after
        position: absolute
        content: ''
        left: 12px
        width: 56px
        bottom: 0
        border-bottom: 1px solid rgba(7,17,27,0.1)
      .text
        display table-cell
        vertical-align middle
        font-size 12px
        font-weight 200
        white-space normal
        line-height 14px
        .iconMap
          vertical-align middle
  .foods-wrapper
    flex 1
    margin-top: 2px;
    .food-list
      h1
        height 26px
        line-height 26px
        padding-left 12px
        font-size 12px
        color rgb(147,153,159)
        background #f3f5f7
        border-left 2px solid #d9dde1
    .food-item
      position relative
      display flex
      margin: 0 18px;
      padding: 18px 0;
      border-bottom 1px solid rgba(7,17,27,0.1)
      .icon
        flex 0 0 57px
      &:last-child
        border-bottom none
      .content
        flex 1
        padding-left 10px
        h2
          margin 2px 0 8px 0
          font-size 14px
          line-height 14px
          height 14px
          font-weight 700
          color rgb(7,17,27)
        .sell-info,.description
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
          .sellCount
            margin-right 4px
        .description
          font-size 10px
          margin-bottom 8px
          line-height: 12px
        .price
          font-size 10px
          font-weight 700
          line-height 24px
          .newPrice
            font-size 14px
            color rgb(240,20,20)
            .unit
              font-size 10px
              font-weight normal
          .oldPrice
            text-decoration line-through
            color rgb(147,153,159)
            padding-left 4px
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom 12px
          z-index 20

</style>
