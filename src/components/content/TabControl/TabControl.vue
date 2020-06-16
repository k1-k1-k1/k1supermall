<template>
  <div class="tab-control">
    <div class="tab-control-item"
         @click="tabClick(index)"
         :class="{active:currentIndex===index}"
         v-for="(item,index) in titles">
     <span> {{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props:{
      titles: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data: function(){
      return{
        currentIndex:0
      }
    },
    methods:{
      tabClick(index){
        this.currentIndex=index;
        this.$emit('tabClick',index)
      }
    }
  }
</script>

<style scoped>
.tab-control{
  height: 44px;
  line-height: 44px;
  display: flex;
  text-align: center;
  font-size: 15px;
  background-color: #fff;
  z-index: 1;
}
  .tab-control-item{
    flex: 1;
  }
  .tab-control-item span{
    padding: 5px;
  }
  .active{
    color:var(--color-high-text)
  }
  .active span{
    border-bottom: 2px solid var(--color-high-text);
  }

</style>
