<template>
  <!-- <h2>分类</h2> -->
  <div class="wrapper">
    <ul>
      <button @click="btnClick">按钮</button>
      <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    <li>aaa</li>
    </ul>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Category",
    mounted() {
      this.scroll = new BScroll(document.querySelector('.wrapper'), {
        probeType: 3,
        pullUpLoad: true
      })

      this.scroll.on('scroll', (position) => {
        console.log(position)
      })

      this.scroll.on('pullingUp', () => {
        console.log('上拉加载更多')
      })
    },
    methods: {
      btnClick() {
        console.log('aaaa')
      }
    },
  }
</script>

<style scoped>
  .wrapper {
    height: 150px;
    background-color: blue;
    overflow: hidden;
  }
</style>