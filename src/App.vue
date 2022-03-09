<template>
  <div id="app">
    <keep-alive>
    <router-view/>
    </keep-alive>
    <main-tab-bar/>
  </div>
</template>

<script>
import MainTabBar from './components/content/mainTabbar/MainTabBar.vue'


export default {
  name: 'App',
  components: {
    MainTabBar
  }
}
</script>

<style>
/* 已经为assets文件路径配置别名，不需要‘./’ */
@import "~assets/CSS/base.css";
</style>
