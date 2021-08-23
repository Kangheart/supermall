<template>
  <div id="cart">
    <nav-bar class="cart-bar">
      <template v-slot:center>
        <div>购物车({{cartLength}})</div>
      </template>
    </nav-bar>

    <scroll class="content" ref="scroll">
      <cart-list></cart-list>
    </scroll>

    <cart-bottom-bar></cart-bottom-bar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar.vue';
  import CartList from './childComps/CartList.vue'
  import Scroll from 'components/common/scroll/Scroll.vue';
  import CartBottomBar from './childComps/CartBottomBar.vue'
  import { mapGetters } from 'vuex'

  export default {
    name: "Cart",
    components: {
      NavBar,
      CartList,
      CartBottomBar,
      Scroll
    },
    computed: {
      ...mapGetters([
        'cartLength'
      ])
    },
    activated() {
      this.$refs.scroll.refresh()
    },
  }
</script>

<style scoped>
  #cart {
    height: 100vh;
  }
  .cart-bar {
    background-color: var(--color-tint);
    color: #fff;
    font-weight: 600;
  }
  .content {
    height: calc(100% - 93px - 40px);
    overflow: hidden;
  }
</style>