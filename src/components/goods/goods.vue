<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(good,index) in goods" class="menu-good" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
            <span v-show="good.type>0" class="icon" :class="classMap[good.type]"></span><span class="text">{{good.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="good in goods" class="food-list food-list-hook">
            <h1 class="title">{{good.name}}</h1>
            <ul>
              <li v-for="food in good.foods" class="food-item">
                <div class="icon">
                  <img width="57" height="57"  :src="food.icon">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartcontrol :food="food" @increment="incrementTotal"></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart  ref="shopCart"
                 :selectFoods="selectFoods"
                 :deliveryPrice="seller.deliveryPrice"
                 :minPrice="seller.minPrice"></shopcart>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcart from '../../components/shopcart/shopcart';
  import cartcontrol from '../../components/cartcontrol/cartcontrol';

  const ERR_OK = 0;

	export default{
	  props:{
	    seller:{
	      type:Object
      }
    },
    components:{
      shopcart,
      cartcontrol
    },
	  data(){
	    return {
	      goods:{},
        listHeight:[],
        scrollY:0
      }
    },
    computed:{
      currentIndex(){
        for(let i = 0;i<this.listHeight.length;i++){
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
              return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        if(this.goods.length){
          this.goods.forEach((good) => {
            good.foods.forEach((food) => {
              if (food.count) {
                foods.push(food);
              }
            });
          });
        }
        return foods;
      }
    },
    created(){
      this.classMap = ['decrease','discount','special','invoice','guarantee'];
      this.$http.get('/api/goods').then(function (response) {
        response = response.body;
        if(response.errno == ERR_OK){
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });

        }
      })
    },
    methods:{
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click:true
        });

        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click:true,
          probeType:3
        });

        this.foodsScroll.on('scroll',(pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight(){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for(let i= 0;i<foodList.length;i++){
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index,event){
        if(!event._constructed){
          return;
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el,300);
      },
      incrementTotal(target) {
        this.$refs.shopCart.drop(target);
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .goods
    position: absolute
    width:100%
    top: 174px
    bottom:46px
    display :flex
    overflow :hidden
    .menu-wrapper
      flex:0 0 88px
      width:80px
      background: #f3f5f7
      .menu-good
        display: table
        height: 54px
        width: 62px
        line-height: 14px
        padding:0 12px
        text-align :center
        &.current
          position:relative
          margin-top: -1px
          z-index :10
          background: #fff
          font-weight :700
        .text
          border:0
        .icon
          display: inline-block
          width: 12px
          height: 12px
          margin: 0 0 -18px 0
          background-size:12px 12px
          background-repeat :no-repeat
          &.decrease
            background-image :url('decrease_1@2x.png')
          &.discount
            background-image :url('discount_1@2x.png')
          &.guarantee
            background-image :url('guarantee_1@2x.png')
          &.invoice
            background-image :url('invoice_1@2x.png')
          &.special
            background-image :url('special_1@2x.png')
        .text
          display: table-cell
          font-size :12px
          width: 56px
          vertical-align: middle;
          border-bottom:1px solid rgba(7,17,27,0.1)
    .foods-wrapper
      flex:1
      .title
        padding-left: 14px
        height: 26px
        line-height: 26px
        border-left:2px solid #d9dde1
        font-size: 12px
        color: rgb(147, 153, 159)
        background: #f3f5f7
      .food-item
        display: flex
        margin:18px
        border-bottom:1px solid rgba(7,17,27,0.1)
        &.last-child
          border-bottom:0
          margin-bottom :0
        .icon
          flex:0 0 57px
          margin-right: 10px
        .content
          flex:1
          .name
            margin: 2px 0 8px 0
            height: 14px
            line-height: 14px
            font-size:14px
            color:rgb(7,17,27)
            white-space: nowrap;
          .desc, .extra
            margin-bottom: 8px
            line-height :10px
            font-size :10px
            color:rgb(147,153,159)
          .desc
            margin-bottom :8px
            line-height :12px
          .extra
            &.count
              margin-right: 12px
          .price
            font-weight:700
            line-weight:24px
          .now
            margin-right:8px
            font-size: 14px
            color:rgb(240,20,20)
          .old
            text-decoration :line-through
            font-size :10px
            color:rgb(147,153,159)
</style>
