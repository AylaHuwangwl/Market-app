<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners" />
    <home-recommend-view :recommends="recommends"></home-recommend-view>
    <feature-view></feature-view>
    <tab-control
      :class="tab - control"
      :titles="['流行', '新款', '精选']"
    ></tab-control>
    <ul>
      <li>猎豹1</li>
      <li>猎豹2</li>
      <li>猎豹3</li>
      <li>猎豹4</li>
      <li>猎豹5</li>
      <li>猎豹6</li>
      <li>猎豹7</li>
      <li>猎豹8</li>
      <li>猎豹9</li>
      <li>猎豹10</li>
      <li>猎豹11</li>
      <li>猎豹12</li>
      <li>猎豹13</li>
      <li>猎豹14</li>
      <li>猎豹15</li>
      <li>猎豹16</li>
      <li>猎豹17</li>
      <li>猎豹18</li>
      <li>猎豹19</li>
      <li>猎豹20</li>
      <li>猎豹21</li>
      <li>猎豹22</li>
      <li>猎豹23</li>
      <li>猎豹24</li>
      <li>猎豹25</li>
      <li>猎豹26</li>
      <li>猎豹27</li>
      <li>猎豹28</li>
      <li>猎豹29</li>
      <li>猎豹30</li>
      <li>猎豹31</li>
      <li>猎豹32</li>
      <li>猎豹33</li>
      <li>猎豹34</li>
      <li>猎豹35</li>
      <li>猎豹36</li>
      <li>猎豹37</li>
      <li>猎豹38</li>
      <li>猎豹39</li>
      <li>猎豹40</li>
      <li>猎豹41</li>
      <li>猎豹42</li>
      <li>猎豹43</li>
      <li>猎豹44</li>
      <li>猎豹45</li>
      <li>猎豹46</li>
      <li>猎豹47</li>
      <li>猎豹48</li>
      <li>猎豹49</li>
      <li>猎豹50</li>
    </ul>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar.vue";
import TabControl from "@/components/content/tabControl/TabControl.vue";

import HomeSwiper from "./childComps/HomeSwiper.vue";
import HomeRecommendView from "./childComps/HomeRecommendView.vue";
import FeatureView from "./childComps/FeatureView.vue";

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
    };
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods('pop');
    // this.getHomeGoods('new');
    // this.getHomeGoods('sell');
  },
  methods: {
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type.page] +1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type],page+=1;
      });
    },
  },
};
</script>

<style scoped>
#home {
  padding-top: 44px;
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
}
</style>
