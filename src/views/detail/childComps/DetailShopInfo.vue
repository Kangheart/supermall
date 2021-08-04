<template>
  <div class="detail-shop-info" v-if="Object.keys(shop).length !== 0">
		<div class="shop-name-info">
			<img :src="shop.logo" alt="" class="shop-img">
			<span>{{shop.name}}</span>
		</div>

		<div class="shop-other-info">
			<div class="shop-other-item shop-other-sell">
				<div>
					<p class="f16">{{shop.sells|saveNum}}</p>
					<p>总销量</p>
				</div>
				<div>
					<p class="f16">{{shop.goodCount}}</p>
					<p>全部宝贝</p>
				</div>
			</div>
			<div class="shop-other-item shop-other-right">
				<div v-for="item in shop.score">
					{{item.name}}&ensp;
					<span class="score" :class="{active:item.isBetter}">{{item.score}}</span>
					<span class="score-bg" :class="{active:item.isBetter}">{{item.isBetter? '高' : '低'}}</span>
				</div>
			</div>
		</div>

		<div class="enter-shop-wrap">
			<div class="enter-shop">进店逛逛</div>
		</div>
	</div>

</template>

<script>
  export default {
    name: 'DetailShopInfo',
    props: {
      shop: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    filters: {
			saveNum(value){
				if(value < 10000) return value;
				return (value/10000).toFixed(1)+'万'
			}
		}
  }
</script>

<style scoped>
  .detail-shop-info {
    font-size: 12px;
  }

  .shop-name-info {
    margin: 15px 10px;
  }
  .shop-img {
    height: 50px;
    width: 50px;
    border-radius: 100%;
    border: 1px solid rgba(128, 128, 128, 0.5);
  }
  .shop-name-info span{
    position: relative;
    font-size: 14px;
    top: -18px;
    margin-left: 20px;
  }

  .shop-other-info {
    display: flex;
    margin-top: -10px;
  }
  .shop-other-info .shop-other-item {
    flex: 1;
    border-right: 1px solid rgba(128, 128, 128, 0.5);
  }
  .shop-other-sell {
    display: flex;
    padding-top: 7px;
    padding-right: 20px;
    padding-left: 20px;
  }
  .shop-other-sell div {
    flex: 1;
    text-align: center;
  }
  .f16 {
    font-size: 16px;
  }
  .shop-other-sell p {
    margin-top: 8px;
  }
  .shop-other-right {
    text-align: left;
    margin-left: 40px;
  }
  .shop-other-right div {
    padding: 5px 0;
  }
  .score {
    display: inline-block;
    width: 30px;
  }
  .shop-other-right span {
    color: rgba(1, 163, 23, 0.644);
  }
  .shop-other-right .active {
    color: rgb(252, 42, 42);
  }

  .enter-shop { 
    width: 150px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    border-radius: 0.42rem;
    background-color: rgba(128, 128, 128, 0.3);
    margin: 10px auto;
  }
</style>