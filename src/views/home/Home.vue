<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentscroll"
      @pulling-up="loadMore"
    >
      <home-swiper :banners="banners" />
      <home-recommend-view :recommends="recommends"></home-recommend-view>
      <feature-view></feature-view>
      <TabControl
        :class="tab - control"
        :titles="['流行', '新款', '精选']"
        @tabclick="tabclick"
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
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");

  },
  mounted(){
     this.$bus.$on("itemImageLoad", () => {
      this.$refs.scroll.refresh();
    });
  },
  methods: {
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
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    contentscroll(position) {
      // console.log(position);
      this.isShowBackTop = -position.y > 1000;
    },
    loadMore() {
      // console.log("上拉记载更多");
      this.getHomeGoods(this.currentType);
      this.$refs.scroll.scroll.refresh();
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
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}
.tab-control {
  position: sticky;
  top: 44px;
  z-index: 9;
}
.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
</style>
