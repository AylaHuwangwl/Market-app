<template>
  <div class="goods-item">
    <img :src="goodsItem.show.img" alt="" @load="imageLoad">
<div class="goods-info">
  <p>{{goodsItem.title}}</p>
  <span class="price">{{goodsItem.price}}</span>
   <span class="collect">{{goodsItem.cfav}}</span>
   </div></div>
</template>

<script>
export default {
name:"GoodsListItem",
props:{
  goodsItem:{
    type:Object,
    default(){
      return {}
    }
  }
},
methods:{
  imageLoad(){
    this.$bus.$emit('itemImageLoad')
  }
}
}
</script>

<style>
.goods-item img{
  width:100%;
  border-radius: 5px;
}
.goods-item{
  padding-bottom: 40px;
  position: relative;
  width: 48%;
}
.goods-info{
font-size:12px;
position: absolute;
bottom: 5px;
left:0;
right:0;
overflow:hidden;
text-align: center;
}
.goods-info p{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 3px;
}
.goods-info .price{
  color:darksalmon;

}
.goods-info .collect{
  position: relative;
  padding-left: 15px;
}
/* 伪元素  */
.goods-info .collect::before{
  content:'';
  position: absolute;
  left:0px;
  top:-1px;
  width:14px;
  height: 14px;
  background: url("@/assets/img/common/collect.svg") 0 0/14px 14px;

}
</style>
