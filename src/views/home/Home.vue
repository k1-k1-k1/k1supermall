<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
<home-swiper :banners="banners"/>
<recommend-view :recommends="recommends"></recommend-view>
  <feature-view/>
<tab-control class ="tabcontrol"
             :titles="['流行','新款','精选']"
             @tabClick="tabClick"/>
  <goods-list :goods-list="showgoods" />

</div>

</template>

<script>

  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";

  import NavBar from "../../components/common/navbar/NavBar";
  import TabControl from "../../components/content/TabControl/TabControl";
  import GoodsList from "../../components/content/goods/GoodsList";

  import {getHomeMultidata,getHomeGoods} from "network/home";
  export default {
    name: "Home",
    components:{
      RecommendView,
      FeatureView,
      HomeSwiper,

      NavBar,
      TabControl,
      GoodsList
    },
    computed:{
      showgoods(){
        return this.goods[this.currentType].list
      }
    },
    data(){
      return{
        banners:[],
        recommends:[],
        goods:{
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]},
        },
        currentType:'pop'
      }
    },
    created() {
      this.getHomeMultidata()
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    methods:{
      getHomeMultidata(){
        getHomeMultidata().then(res=>{
          // console.log(res);
          this.banners=res.data.banner.list;
          this.recommends=res.data.recommend.list;
        })
      },
      getHomeGoods(type){
        const page=this.goods[type].page+1
        getHomeGoods(type,page).then(res=>{
          // console.log(res.data)
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page+=1
        })
      },
      tabClick(index){
        switch (index) {
        case  0:
        this.currentType='pop'
            break
          case  1:
            this.currentType='new'
            break
          case  2:
            this.currentType='sell'
            break
        }
      }
    }
  }
</script>

<style scoped>
  #home{
    padding-top: 44px;
    padding-bottom: 1000px;
  }
.home-nav{
  background-color: var(--color-tint);
  color: #fff;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}
  .tabcontrol{
    position: sticky;
    top: 44px;
    z-index: 1;
  }
</style>
