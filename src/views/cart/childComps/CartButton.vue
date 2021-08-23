<template>
  <div class="cart-button">
    <img src="../../../assets/img/cart/tick.svg" alt="" :class="{ checked: isChecked }">
  </div>
</template>

<script>
export default {
  name: "CartButton",
  props: {
    isChecked: {
      type: Boolean,
      // default: false
    }
  }
}
</script>

<style scoped>
  .cart-button {
    width: 25px;
  }
  .cart-button img{
    /* background-color: blue; */
    /* color: rgb(156, 39, 39); */
    margin-left: 2.5px;
    border-radius: 50%;
    margin-top: 28px;
    border: 1.5px solid gray;
    text-align: center;
  }
  .checked {
    background-color: red;
  }
</style>