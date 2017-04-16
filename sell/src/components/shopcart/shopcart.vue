<template lang="html">
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
          <div class="badge" v-show="totalCount>0">{{totalCount}}</div>
					<div class="fuck">
             1231四渡赤水
					</div>
					<div class="logo">
						<i class="icon-shopping_cart"></i>
					</div>
				</div>
				<div class="price">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay">
					￥{{minPrice}}元起送
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [
					{
						totalCount: 10
					}
				]
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      totalPrice: 0
    }
  },
  computed: {
    totalPrice() {
      let total = 0
      this.selectFoods.each((food) => {
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
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.shopcart
		position: fixed
		left: 0
		bottom: 0
		z-index: 50
		width: 100%
		height: 48px
		.content
			display: flex
			background: #141d27
			font-size: 0
			.content-left
				flex: 1
				.logo-wrapper
					display: inline-block
					margin: 0 12px
					padding: 6px
					position: relative
					top: -10px
					width: 56px
					height: 56px
					box-sizing: border-box
					vertical-align: top
					border-radius: 50%
					background: #141d27
          .badge
						position: absolute
						top: 0
						right: 0
						background: rgb(240,20,20)
						color: #fff
						width: 24px
						height: 16px
						line-height: 16px
						font-size: 9px
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
						font-weight: 700
						border-radius: 16px
						text-align: center
					.logo
						width: 100%
						height: 100%
						border-radius: 50%
						background: #2b343c
						text-align: center
						.icon-shopping_cart
							font-size: 24px
							color: #80858a
							line-height: 44px
				.price
					display: inline-block
					vertical-align: top
					margin-top: 12px
					padding-right: 12px
					box-sizing: border-box
					line-height: 24px
					border-right: 1px solid rgba(255,255,255,0.1)
					font-size: 16px
					font-weight: 700
					color: rgba(255,255,255,0.4)
				.desc
					display: inline-block
					margin: 12px 0 0 12px
					vertical-align: top
					line-height: 24px
					color: rgba(255,255,255,0.4)
					font-size: 10px
			.content-right
				flex: 0 0 105px
				width: 105px
				text-align: center
				background: #2b333b
				.pay
					height: 48px
					line-height: 48px
					text-align: center
					font-size: 12px
					color: rgba(255,255,255,0.4)
					font-weight: 700
					background: #2b333b
</style>
