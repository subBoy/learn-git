<template>
  <div class="food-detail">
    <transition name="move">
      <div class="detail" v-show="showFlag" ref="foodDetail">
        <div class="food-content">
          <div class="img-header">
            <img class="pic" :src="food.image">
            <div class="go-back">
              <i class="icon-arrow_lift" @click="detailHide"></i>
            </div>
          </div>
          <div class="food-desc">
            <h3 class="name">{{food.name}}</h3>
            <p class="food-count-rating">
              <span class="sellCount">月售{{food.sellCount}}份</span>
              <span class="rating">好评率{{food.rating}}%</span>
            </p>
            <p class="food-price">
              <span class="now">￥<em>{{food.price}}</em></span>
              <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
            </p>
            <div class="addCart-btn">
              <div class="addCart" v-show="!food.count" @click="addfood"><p class="addCart-text">加入购物车</p></div>
              <div class="cartcontrol-wrapper" v-show="food.count > 0">
                <cartcontrol @cartadd="cartadd" :food="food"></cartcontrol>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import Vue from 'vue';
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  export default {
    props: {
      food: {
        type: Object,
        default: {}
      },
      cartadd: {
        type: Function
      }
    },
    data () {
      return {
        showFlag: false
      };
    },
    created () {
      this.$nextTick(() => {
        this._detailScroll();
      });
    },
    methods: {
      detailShow () {
        this.showFlag = true;
      },
      detailHide () {
        this.showFlag = false;
      },
      _detailScroll () {
        this.detailScroll = new BScroll(this.$refs.foodDetail, {
          click: true
        });
      },
      addfood (event) {
        if (!event._constructed) {
          return;
        };
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        }
      }
    },
    components: {
      cartcontrol
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .food-detail
    .detail
      position: fixed
      left: 0
      top: 0
      bottom: 48px;
      width: 100%
      z-index: 30
      background: #f3f5f7
      transition: all .2s linear
      transform: translate3d(0, 0, 0)
      &.move-enter, &.move-leave-active
        transform: translate3d(100%, 0, 0)
      .food-content
        .img-header
          position: relative
          width: 100%
          height: 0
          padding-top: 100%
          .pic
            position: absolute
            top: 0
            left: 0
            width: 100%
            height: 100%
          .go-back
            position: absolute
            left: 0
            top: 10px
            .icon-arrow_lift
              padding: 10px
              font-size: 20px
              color: #f5f5f5
        .food-desc
          position: relative
          padding: 18px
          border-1px(rgba(7, 17, 27, 0.1))
          margin-bottom: 16px
          background: #fff
          .name
            font-size: 14px
            font-weight: 700
            color: rgb(7, 17, 27)
            line-height: 14px
            margin-bottom: 8px
          .food-count-rating
            font-size: 0
            margin-bottom: 18px
            .sellCount
              display: inline-block
              margin-right: 12px
              font-size: 10px
              color: rgb(147, 158, 159)
              line-height: 10px
            .rating
              display: inline-block
              margin-right: 12px
              font-size: 10px
              color: rgb(147, 158, 159)
          .food-price
            font-size: 0
            .now
              display: inline-block
              font-size: 10px
              font-weight: 700
              color: rgb(240, 20, 20)
              line-height: 24px
              font-weight: 700
              font-family: "微软雅黑"
              em
                display: inline-block
                font-style: normal
                font-size: 14px
                margin-right: 12px
                font-weight: 700
            .old
              display: inline-block
              font-size: 10px
              color: rgb(147, 153, 159)
              line-height: 24px
              font-weight: 700
              font-family: "微软雅黑"
              text-decoration: line-through
          .addCart-btn
            position: absolute
            right: 18px
            bottom: 18px
            .addCart
              width: 74px
              height: 24px
              background: rgb(0, 160, 220)
              border-radius: 12px
              .addCart-text
                font-size: 10px
                line-height: 12px
                padding: 6px 0
                color: #fff
                text-align: center
                vertical-align: middle
</style>
