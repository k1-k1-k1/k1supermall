<template>
<div class="goodlist">
<goods-list-item v-for="(item,index) in goodsList"
                 :key="index"
                 :goods="item"/>
<!--  多了个东西，搞了一天bug-->
<!--  {{goodsList}}-->
</div>

</template>

<script>
  import GoodsListItem from "./GoodsListItem";
  export default {
    name: "GoodsList",
    components:{
      GoodsListItem
    },
    props:{
      goodsList:{
        type:Array,
        default(){
          return []
        }
      }
    }
  }
</script>

<style scoped>
  .goodlist{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
  }

</style>
