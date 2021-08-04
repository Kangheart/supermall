<template>
  <div v-if="Object.keys(goods).length !== 0" class="base-info">
    <div class="info-title">{{goods.title}}</div>
    <div class="price-wrap">
			<div class="new-price">{{goods.newPrice}}</div>
			<div class="old-price">{{goods.oldPrice}}</div>
			<div class="discount-desc" v-if="goods.discount">{{goods.discount}}</div>
		</div>
    <div class="other-info">
			<div>{{goods.columns[0]}}</div>
			<div>{{goods.columns[1]}}</div>
			<div>{{goods.services[goods.services.length-1].name}}</div>
		</div>
		<div class="serves-wrap">
			<div v-for="index in goods.services.length-1" :key="index" class="serves-wrap-item"> 
				<img :src="goods.services[index-1].icon" alt=""> 
				<span>{{goods.services[index-1].name}}</span>
			</div>
		</div>
  </div>
</template>

<script>
  export default {
    name: 'DetailBaseInfo',
    props: {
      goods: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .base-info {
    /* margin-top: 10px; */
    padding: 0 8px;
    color: #999;
    border-bottom: 5px solid #f2f5f8;
  }

  .info-title {
    color: #222;
    margin: 10px 0;
		padding: 0 4px;
		font-size: 14px;
		line-height: 20px;
  }

  .price-wrap {
		display: flex;
		padding:0 4px 10px;
	}
  .price-wrap .new-price {
    font-size: 14px;
		color: var(--color-high-text)
	}
  .price-wrap .old-price {
		color: #999;
		font-size: 12px;
		margin: 0 4px;
		text-decoration: line-through;
	}
	.price-wrap .discount-desc {
		color: #fff;
		font-size: 12px;
		border-radius: 100px;
		padding: 2px 6px;
		background-color: var(--color-high-text);
	}

	.other-info {
		display: flex;
		color: #999;
		font-size: 12px;
		padding: 7px 4px;
		border-bottom: 1px solid #ededed;
		border-top: 1px solid #ededed;
	}
  .other-info div {
		flex: 1;
	}
  
	.serves-wrap {
		display: flex;
		font-size: 12px;
		color: #333;
		padding: 10px 4px;
	}
  .serves-wrap-item {
    flex: 1;
  }
  .serves-wrap-item img {
		width: 12px;
		height: 12px;
    margin-bottom: -1.5px;
	}
</style>