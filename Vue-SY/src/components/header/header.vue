<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name" v-text="seller.name"></span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text" v-text="seller.supports[0].description"></span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class='icon-keyboard_arrow_right'></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span>{{seller.bulletin}}
      <i class='icon-keyboard_arrow_right'></i>
    </div>
    <div class="header-bg">
      <img :src="seller.avatar" width="100%" height="100%" alt="">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name" v-text="seller.name"></h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="bor-line"></div>
              <div class="title-text">优惠信息</div>
              <div class="bor-line"></div>
            </div>
            <ul class="supports" v-if="seller.supports">
              <li class="support-item" v-for="(item, index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text" v-text="seller.supports[index].description"></span>
              </li>
            </ul>
            <div class="title ">
              <div class="bor-line"></div>
              <div class="title-text">商家公告</div>
              <div class="bor-line"></div>
            </div>
            <p class="active" v-text="seller.bulletin"></p>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import star from 'components/star/star';

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail () {
        this.detailShow = true;
      },
      hideDetail () {
        this.detailShow = false;
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>


<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .header
    position: relative
    color: #fff
    background: rgba(7, 17, 27, 0.5)
    overflow: hidden
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display: inline-block
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-img('brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            font-size: 16px
            padding-left: 6px
            line-height: 18px
            font-weight: bold
        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
        .supports
          .icon
            display: inline-block
            vertical-align: top
            height: 12px
            width: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-img("decrease_1")
            &.discount
              bg-img("discount_1")
            &.guarantee
              bg-img("guarantee_1")
            &.invoice
              bg-img("invoice_1")
            &.special
              bg-img('special_1')
          .text
            vertical-align: middle
            display: inline-block
            line-height: 12px
            font-size: 10px
      .support-count
        position: absolute
        right: 12px
        bottom: 18px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        font-size: 0
        .count
          display: inline-block
          vertical-align: top
          font-size: 10px
          line-height: 25px
        .icon-keyboard_arrow_right
          display: inline-block
          margin-left: 2px
          line-height: 25px
          font-size: 10px
    .bulletin-wrapper
      height: 28px
      line-height: 29px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      position: relative
      background-color: rgba(7, 17, 27, 0.2)
      font-size: 10px
      .bulletin-title
        vertical-align: top
        display: inline-block
        margin: 8px 4px 0 0
        width: 22px
        height: 12px
        bg-img('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .icon-keyboard_arrow_right
        display: inline-block
        position: absolute
        font-size: 10px
        right: 12px
        top: 9px
    .header-bg
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    .detail
      position: fixed
      top: 0
      left: 0
      z-index: 100
      width: 100%
      height: 100%
      overflow: auto
      transition: all .3s
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10px)
      &.fade-enter-active, &.fade-leave
        opacity: 1
        background: rgba(7, 17, 27, 0.8)
      &.fade-enter, &.fade-leave-active
        opacity: 0
        background: rgba(7, 17, 27, 0.1)
      .detail-wrapper
        min-height: 100%
        width: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            font-size: 16px
            font-weight: 700
            color: #fff
            text-align: center
            padding-bottom: 16px
          .star-wrapper
            text-align: center
            padding: 2px 0;
            margin-bottom: 28px
          .title
            display: flex
            width: 80%
            margin: 0 auto;
            font-size: 0
            .bor-line
              flex: 1
              position: relative
              top: -6px
              border-bottom: 1px solid rgba(255, 255, 255, 0.2)
            .title-text
              font-size: 14px
              font-weight: 700
              color: #fff
              line-height: 14px
              padding: 0 12px
          .supports
            width: 80%
            margin: 0 auto
            padding: 24px 0 28px 0
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &:last-child
                margin-bottom: 0
              .icon
                display: inline-block
                width: 16px
                height: 16px
                vertical-align: top
                margin-right: 6px
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-img("decrease_2")
                &.discount
                  bg-img("discount_2")
                &.guarantee
                  bg-img("guarantee_2")
                &.invoice
                  bg-img("invoice_2")
                &.special
                  bg-img('special_2')
              .text
                display: inline-block
                line-height: 16px
                font-size: 12px
          .active
            width: 76%
            margin: 0 auto
            padding-top: 24px
            font-size: 12px
            line-height: 24px
            font-weight: 200
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto
        clear: both
        font-size: 32px
</style>
