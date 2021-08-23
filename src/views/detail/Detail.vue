<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
    <scroll class="content" ref="scroll" :probe-type = "3" @scroll="contentScroll">
      <detail-swiper :top-images="topImages"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info :detailInfo="detailInfo" @loadImgEvent="loadImgEvent"></detail-goods-info>
      <detail-params ref="params" :paramInfo="paramInfo"></detail-params>
      <detail-comment-info ref="comment" :commentInfo="commentInfo"></detail-comment-info>
      <goods-list ref="recommend" :goods="recommend"></goods-list>
    </scroll>
    <detail-bottom-bar @addEvent="addEvent"></detail-bottom-bar>
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar.vue'
  import DetailSwiper from './childComps/DetailSwiper.vue'
  import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
  import DetailShopInfo from './childComps/DetailShopInfo.vue'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
  import DetailParams from './childComps/DetailParams.vue'
  import DetailCommentInfo from './childComps/DetailCommentInfo.vue'
  import DetailBottomBar from './childComps/DetailBottomBar.vue'

  import Scroll from 'components/common/scroll/Scroll.vue';
  import GoodsList from 'components/content/goods/GoodsList.vue'
  import BackTop from 'components/content/backTop/BackTop.vue'

  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from 'network/detail.js';
  import {debounce} from 'common/utils.js'

  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParams,
      DetailCommentInfo,
      Scroll,
      GoodsList,
      DetailBottomBar,
      BackTop
    },
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommend: [],
        themeTopYs: [],
        getThemeTopYs: null,
        currentIndex: [],
        isShowBackTop: false,
        message: '',
        show: false
      }
    },
    created() {
      //1.保存传入的iid
      this.iid = this.$route.params.iid

      //2.根据iid请求详情数据
      getDetail(this.iid).then(res => {
        const data = res.result
        //1.轮播图
        // console.log(res);
        this.topImages = data.itemInfo.topImages;

        //2.商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services);

        //3.商家信息
        this.shop = new Shop(data.shopInfo);

        //4.商品的详情数据
        this.detailInfo = data.detailInfo;

        //5.获取参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule);

        //6.评论信息
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0];
        }

        // this.$nextTick(() => {
        //   this.themeTopYs = []

        //   this.themeTopYs.push(0);
        //   this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        //   this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        //   this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);

        //   console.log(this.themeTopYs)
        // })
      })
      
      //3.请求推荐数据
      getRecommend().then(res => {
        // console.log(res);
        this.recommend = res.data.list
      })

      this.getThemeTopYs = debounce(() => {
        this.themeTopYs = []
        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);

        console.log(this.themeTopYs)
      }, 100)
    },
    methods: {
      loadImgEvent() {
        this.$refs.scroll.refresh();

        this.getThemeTopYs()
      },
      titleClick(index) {
        console.log(index);
        this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
      },
      contentScroll(position) {
        const positionY = -position.y;
        let length = this.themeTopYs.length;
        for(let i = 0; i < length; i++) {
          if(this.currentIndex != i && 
              ((i < length - 1 && positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1]) 
              || (i === length - 1 && positionY >= this.themeTopYs[i]))) {
            this.currentIndex = i;
            this.$refs.nav.currentIndex = this.currentIndex;
          }
        }

        this.isShowBackTop = (-position.y) > 1000
      },
      backClick() {
        this.$refs.scroll.scrollTo(0, 0, 500)
      },
      addEvent() {
        // 1.获取购物车需要展示的信息
        const product = {};
        product.image = this.topImages[0];
        product.title = this.goods.title;
        product.desc = this.goods.desc;
        product.price = this.goods.realPrice;
        product.iid = this.iid;

        //2.将商品添加到购物车中
        // this.$store.commit('addCart', product)
        this.$store.dispatch('addCart', product).then(res => {
          // this.message = res;
          // this.show = true
          
          // setTimeout(() => {
          //   this.show = false;
          //   this.message = ''
          // }, 1000)
          this.$toast.show(res, 2000)
          console.log(this.$toast)
        })
      }
    },
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .content {
    height: calc(100% - 44px);
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
</style>
