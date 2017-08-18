<template>
	<div class="header">
		  <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" :src="seller.avatar">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
        </div>
      </div>
		  <div class="bulletin-wrapper" @click="showDetail">
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      </div>
      <div class="background">
        <img :src="seller.avatar" width="100%" height="100%">
      </div>
      <transition name="myFade">
          <div v-show='detailShow' class="detail">
              <div class="detail-wrapper">
                  <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                  </div>
                  <div class="title">
                    <div class="line"></div>
                    <div class="text">优惠信息</div>
                    <div class="line"></div>
                  </div>
                  <ul v-if="seller.supports" class="supports">
                    <li class="support-item" v-for="item in seller.supports">
                      <span class="icon" :class="classMap[item.type]"></span>
                      <span class="text">{{item.description}}</span>
                    </li>
                  </ul>
                <div class="title">
                  <div class="line"></div>
                  <div class="text">商家公告</div>
                  <div class="line"></div>
                </div>
                <div class="bulletin">
                  <p class="content">{{seller.bulletin}}</p>
                </div>
              </div>
              <div class="detail-close" @click="hideDetail">
              </div>
          </div>
      </transition>
	</div>
</template>
<script type="text/javascript">
	export default{
	  props: {
	    seller: {
	      type: Object
      }
    },
    data(){
      return {
        detailShow:false
      }
    },
    methods:{
      showDetail(){
        this.detailShow = true;
      },
      hideDetail(){
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease','discount','special','invoice','guarantee'];
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .header
    position :relative
    color:#fff
    background :rgba(7,17,27,0.5)
    overflow :hidden
    .content-wrapper
      position:relative
      padding:24px 12px 18px 24px
      font-size :0
      .avatar
          display : inline-block
          vertical-align :top
          img
            border-radius :2px
      .content
          display : inline-block
          font-size :14px
          margin-left:16px
          .title
            margin: 2px 0 8px 0
            .brand
              display :inline-block
              vertical-align :top
              width: 30px
              height: 18px
              background-image :url("brand@2x.png")
              background-size : cover
              background-repeat : no-repeat
            .name
              margin-left:6px
              font-size :18px
              line-height :18px
              font-weight :bold
          .description
              margin-top: 10px
              margin-bottom: 10px
              line-height: 12px
              font-size :16px
          .support
            .icon
              display:inline-block
              vertical-align :top
              width: 14px
              height: 14px
              margin-right: 4px
              background-size :14px 14px
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
              line-height :12px
              font-size :14px
              vertical-align :top
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        padding: 0 8px
        height: 24px
        line-height :24px
        border-radius :14px
        background :rgba(0,0,0,0.2)
        text-align:center
      .count
          font-size :10px
    .bulletin-wrapper
      height: 28px
      line-height: 28px
      padding:0 22px 0 12px
      white-space :nowrap
      overflow :hidden
      text-overflow :ellipsis
      background :rgba(7,17,27,0.2)
      .bulletin-title
        display: inline-block
        vertical-align :top
        margin-top :8px
        width: 22px
        height:12px
        background-image :url('bulletin@2x.png')
        background-size :22px 12px
        background-repeat :no-repeat
      .bulletin-text
        vertical-align :top
        margin: 0 4px
        font-size :10px
    .background
      position :absolute
      top:0
      left:0
      width:100%
      height:100%
      z-index :-1
      filter:blur(10px)
    .detail
      position: fixed
      top:0
      left: 0
      z-index:100
      width:100%
      height:100%
      overflow:auto
      transition :all 0.5s
      background: rgba(7,17,27,0.8)
      backdrop-filter:blur(10px)
      &.myFade-transition
        opacity :1
        background: rgba(7,17,27,0.8)
      &.myFade-enter,&.myFade-leave
        opacity:0
        background: rgba(7,17,27,0)
      .detail-wrapper
        display: inline-block
        min-height :100%
        width:100%
        margin-top :64px
        padding-bottom: 64px
        .detail-main
          .name
            line-height: 16px
            text-align:center
            font-size: 16px
        .title
          display: flex
          width:80%
          margin:28px auto 24px auto
          .line
            flex:1
            position: relative
            top: -6px
            border-bottom:1px solid rgba(255,255,255,0.2)
          .text
            padding:0 12px
            font-weight:700
            font-size :14px
        .supports
          width:80%
          margin:0 auto
          .support-item
            padding:0 12px
            margin-bottom: 12px
            font-size: 0
            &:last-child
              margin-bottom:0
            .icon
              display: inline-block
              width: 16px
              height: 16px
              vertical-align: top
              margin-right: 6px
              background-size:16px 16px
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
              line-height :12px
              font-size :12px
        .bulletin
          width:80%
          margin:0 auto
          .content
            padding:0 12px
            line-height: 24px
            font-size:12px
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto
        clear:both
        font-size:32px
        background-image :url('close.png')
        background-size :32px 32px
        background-repeat :no-repeat
</style>
