<template>
  <div class="bottom-bar">
    <div class="check">
      <cart-button class="button" :isChecked="isChecked" @click.native="checkClick"></cart-button>
      <span>全选</span>
    </div>

    <div class="price">
      合计：{{totalPrice}}
    </div>

    <div class="count">
      <span>去支付({{totalCount}})</span>
    </div>
  </div>
</template>

<script>
  import CartButton from './CartButton.vue'

  export default {
    name: "CartBottomBar",
    components: {
      CartButton
    },
    computed: {
      totalPrice() {
        return '￥' + this.$store.state.cartList.filter(item => {
          return item.checked
        }).reduce((preValue, item) => {
          return preValue + item.price * item.count
        }, 0)
      },
      totalCount() {
        return this.$store.state.cartList.filter(item => item.checked).length
      },
      isChecked() {
        if(this.$store.state.cartList.length === 0) return false
        // return !(this.$store.state.cartList.filter(item => !item.checked).length)  //效率没那么高
        return !(this.$store.state.cartList.find(item => !item.checked))

        //使用遍历
        // for(let item of this.$store.state.cartList) {
        //   if(!item.checked) {
        //     return false
        //   }
        // }
        // return true
      }
    },
    methods: {
      checkClick() {
        if(this.isChecked) {
          this.$store.state.cartList.forEach(item => item.checked = false);
        } else {
          this.$store.state.cartList.forEach(item => item.checked = true);
        }
      }
    },
  }
</script>

<style scoped>
  .bottom-bar {
    /* position: relative; */
    /* margin-bottom: 49px; */
    display: flex;
    background-color: #eee;
    height: 40px;
  }

  .check {
    display: flex;
    margin-left: 10px;
    width: 70px;
  }

  .check span {
    line-height: 40px;
    margin-left: 3px;
    font-size: 14px;
  }

  .button {
    position: relative;
    top: -18px;
  }

  .price {
    flex: 1;
    line-height: 40px;
    font-size: 14px;
  }

  .count {
    line-height: 40px;
    width: 80px;
    background-color: rgba(245, 58, 214, 0.959);
  }
  .count span {
    position: relative;
    margin-left: 6px;
    font-weight: bolder;
    color: #fff;
  }
</style>