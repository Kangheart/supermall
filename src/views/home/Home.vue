<template>
  <div id="home">

    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>购物街</div>
      </template>
    </nav-bar>
    
    <tab-control :titles="['流行', '新款', '精选']" 
                   @tabClick="tabClick"
                   ref="tabControl1" 
                   class="tab-control"
                   v-show="isTabFixed"></tab-control>

    <scroll class="content" 
            ref="scroll" 
            :probe-type="3" 
            @scroll='contentScroll' 
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control :titles="['流行', '新款', '精选']" 
                   @tabClick="tabClick"
                   ref="tabControl2"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper.vue';
  import RecommendView from './childComps/RecommendView.vue';
  import FeatureView from './childComps/FeatureView.vue';

  import NavBar from 'components/common/navbar/NavBar.vue';
  import Scroll from 'components/common/scroll/Scroll.vue';
  import TabControl from 'components/content/TabControl/TabControl.vue';
  import GoodsList from 'components/content/goods/GoodsList';
  import BackTop from 'components/content/backTop/BackTop.vue'

  import {getHomeMultidata, getHomeGoods} from 'network/home.js';
  import {debounce} from 'common/utils.js'


  export default {
    name: "Home",
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isShowBackTop: false,
        tabOffsetTop: 0,
        isTabFixed: false,
        saveY: 0
      }
    },
    created() {  //组件创建完后立刻发送网络请求
      //1.请求多个数据
      this.getHomeMultidata();

      //2.请求商品数据
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
    mounted() {
      //1.监听item中图片加载完成
      const refresh = debounce(this.$refs.scroll.refresh, 100)
      this.$bus.$on('itemImageLoad', () => {
        refresh()
      })

      //2.获取tabControl的offsetTop
      //所有组件都有一个属性$el：用于获取组件中的元素
      // tabOffsetTop = this.$refs.tabControl.$el
      // console.log(this.$refs.tabControl.$el.offsetTop)
    },
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      this.$refs.scroll.refresh()
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    methods: {
      //事件监听相关
      tabClick(index) {
        switch(index) {
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell';
            break;
        };
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
      },
      backClick() {
        this.$refs.scroll.scrollTo(0, 0, 500)
      },
      contentScroll(position) {
        //1.判断BackTop是否显示
        // console.log(position)
        this.isShowBackTop = (-position.y) > 1000

        //2.决定tabControl是否吸顶(position: fixed)
        this.isTabFixed = (-position.y) > (this.tabOffsetTop)
      },
      loadMore() {
        console.log("上拉到底部")
        this.getHomeGoods(this.currentType)
      },
      swiperImageLoad() {
        // console.log(this.$refs.tabControl.$el.offsetTop)
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
      },
      
      //网络请求相关
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          // console.log(res);
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1;
        getHomeGoods(type, page).then(res => {
          // console.log(res);
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1;
          
          //完成上拉加载更多
          this.$refs.scroll.finishPullUp()
        })
      },
    },
  }
</script>

<style scoped>
  #home {
    /* padding-top: 44px; */
    height: 100vh;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /* 在使用浏览器原生滚动时，为了让导航栏不跟随一起滚动 */
    /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
  }

  .content {
    /* height: calc(100% - 49px); */
    position: absolute;
    top: 44px;
    bottom: 49px;
    overflow: hidden;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }
</style>