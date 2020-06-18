<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
<scroll class="content"
        ref="scroll"
        @scroll="contentScroll"
        @pullingUp="loadMore"
        :data="showgoods"
        :pull-up-load="true"
        :probe-type="3">
  <div>
    <home-swiper :banners="banners" ref="hSwiper"/>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view/>
    <tab-control class ="tabcontrol"
                 :titles="['流行','新款','精选']"
                 @tabClick="tabClick"/>
    <goods-list :goods-list="showgoods" />

  </div>
</scroll>
  <back-top @click.native="backTop"
            v-show="showBackTop"/>
</div>

</template>
<script>
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";
  // import BScroll from 'better-scroll'
  import NavBar from "../../components/common/navbar/NavBar";
  import TabControl from "../../components/content/TabControl/TabControl";
  import GoodsList from "../../components/content/goods/GoodsList";
  import Scroll from "../../components/common/scroll/Scroll";
  import BackTop from "../../components/content/backTop/BackTop";

  import {getHomeMultidata,getHomeGoods} from "network/home";
  export default {
    name: "Home",
    components:{
      RecommendView,
      FeatureView,
      HomeSwiper,

      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
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
        currentType:'pop',
        showBackTop: false
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
          this.$refs.scroll.finishPullUp()
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
      },
      contentScroll(position) {
        // 2.决定backTop是否显示
        this.showBackTop = (-position.y) >1000
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
        this.$refs.scroll.refresh()
      },
      backTop() {
        this.$refs.scroll.scrollTo(0, 0)
      },
    }
  }
</script>

<style scoped>
  #home{
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
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
  .content{
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top: 44px;
  }
  /*.content{*/
  /*  top: 44px;*/
  /*  bottom: 49px;*/
  /*  left: 0;*/
  /*  right: 0;*/
  /*  overflow: hidden;*/
  /*  position: absolute;*/
  /*}*/
</style>
