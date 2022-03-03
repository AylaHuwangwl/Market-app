<template>
  <div id = "home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"/>
    <home-recommend-view :recommends="recommends"></home-recommend-view>
  </div>
</template>

<script>
import NavBar from '@/components/common/navbar/NavBar.vue'
import HomeSwiper from './childComps/HomeSwiper.vue'
import HomeRecommendView from './childComps/HomeRecommendView.vue'

import {getHomeMultidata} from '@/network/home';
// import Swiper from '@/components/common/swiper/Swiper.vue';
// import SwiperItem from '@/components/common/swiper/SwiperItem.vue'
// import {Swiper,SwiperItem} from '@/components/common/swiper'
// import SwiperItem from '@/components/common/swiper/SwiperItem.vue';
  export default {
    name: "Home",
    components:{
      NavBar,
      HomeSwiper,
      HomeRecommendView,
      // Swiper,
      // SwiperItem,
    },
    data(){
      return {
        banners:[],
        recommends:[],
      }
    },
    created() {
      getHomeMultidata().then(res =>{
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    }
  }
</script>

<style scoped>
.home-nav{
  background-color: #ff8198;
  color:#fff;

}

</style>
