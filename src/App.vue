<template>
  <div id="app">
    <wHeader :seller="seller"></wHeader>
    <div class="tab">
    <div class="tab-item">
      <router-link  to="/goods">商品</router-link>
    </div>
    <div class="tab-item">
      <router-link  to="/ratings">评论</router-link>
    </div>
    <div class="tab-item">
      <router-link  to="/seller">商家</router-link>
    </div>
  </div>
    <!--<button @click="jump" style="font-size: 40px">jump</button>-->
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
import header from "@/components/header/header"
import shopcart from "@/components/shopcart/shopcart"

const ERR_OK = 0;

export default {
  name: 'app',
  components:{
    'wHeader':header,
    'shopcart':shopcart
  },
  data() {
    return{
          seller:{}
    };
  },
  created: function (){
    this.$http.get('/api/seller').then(response => {
      response = response.body;
      if(response.errno === ERR_OK){
        this.seller = response.data;
      }


  }, response => {
      // error callback
    });
  },
  methods:{
    jump:function () {
      this.$router.push({path: '/ratings'})
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
  #app
    .tab
      display:flex
      width:100%
      height:40px
      line-height:40px
      .tab-item
        flex:1
        text-align:center
        & > a
          display:block
          text-decoration:none
          font-size: 20px
          color: rgb(77,85,93)
          &.active
            color:rgb(240,20,20)

</style>
