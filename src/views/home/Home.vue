<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <TabControl
        :titles="['流行', '新款', '精选']"
        @tabclick="tabclick"
        ref='tabControl1'
        class="tab-control"
        v-show="isTabFixed"
/>
    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentscroll"
      :pull-up-load="true"
      @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swiperImageLoad = "swiperImageLoad " />
      <home-recommend-view :recommends="recommends"></home-recommend-view>
      <feature-view></feature-view>
      <TabControl
        :titles="['流行', '新款', '精选']"
        @tabclick="tabclick"
        ref='tabControl2'
      />
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top
      @click.native="backClick"
      v-show="isShowBackTop"
      :pull-up-load="true"
    ></back-top>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar.vue";
import TabControl from "@/components/content/tabControl/TabControl.vue";
import GoodsList from "@/components/content/goods/GoodsList.vue";
import BackTop from "@/components/content/backTop/BackTop.vue";

import HomeSwiper from "./childComps/HomeSwiper.vue";
import HomeRecommendView from "./childComps/HomeRecommendView.vue";
import FeatureView from "./childComps/FeatureView.vue";
import Scroll from "@/components/common/scroll/Scroll.vue";

import { getHomeMultidata, getHomeGoods } from "@/network/home";
import {debounce} from "@/common/utils.js"
// import Swiper from '@/components/common/swiper/Swiper.vue';
// import SwiperItem from '@/components/common/swiper/SwiperItem.vue'
// import {Swiper,SwiperItem} from '@/components/common/swiper'
// import SwiperItem from '@/components/common/swiper/SwiperItem.vue';
export default {
  name: "Home",
  components: {
    HomeSwiper,
    HomeRecommendView,
    FeatureView,
    NavBar,
    TabControl,
    GoodsList,
    Scroll,
    BackTop,
    // Swiper,
    // SwiperItem,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
      isShowBackTop: false,
      tabOffsetTop:0,
      isTabFixed:false,
      saveY:0
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
    destroyed(){
      console.log("destroyed");
    },
    activated(){
      console.log("active");
    },
    deactivated(){
      console.log("deactive");
    }
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");

  },
  mounted(){
     const refresh = debounce(this.$refs.scroll.refresh,50);
     this.$bus.$on("itemImageLoad", () => {
      //  封装一个定时器在执行refresh之前先等待一会（防抖）
      // this.$refs.scroll.refresh();
      // refresh是局部变量但不会销毁，因为在闭包中使用了
     refresh();
    });

  },
  methods: {
    // 事件监听相关方法
    swiperImageLoad(){
    // console.log(this.$refs.tabControl.$el.offsetTop);
    this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    },
    tabclick(index) {
      // console.log(index);
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
      this.$refs.tabControl2.currentType = index;
       this.$refs.tabControl1.currentType = index;
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    contentscroll(position) {
      // console.log(position);
      // 1、判断BackTop是否显示
      this.isShowBackTop = -position.y > 1000;
      // 2、决定tabControl是否吸顶
      this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
    loadMore() {
      // console.log("上拉记载更多");
      this.getHomeGoods(this.currentType);
      // this.$refs.scroll.scroll.refresh();
    },
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        // 完成上拉加载更多
        this.$refs.scroll.finishPullUp();
      });
    },
  },
};
</script>

<style scoped>
#home {
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: #ff8198;
  color: #fff;
  /* position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9; */
}
.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
.tab-control{
  position: relative;
  z-index: 9;
}
</style>
