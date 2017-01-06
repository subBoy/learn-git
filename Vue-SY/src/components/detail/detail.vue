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
              <transition name="fade">
                <div class="addCart" v-show="!food.count || food.count === 0" @click="addfood">加入购物车</div>
              </transition>
              <div class="cartcontrol-wrapper" v-show="food.count > 0">
                <cartcontrol @cartadd="cartadd" :food="food"></cartcontrol>
              </div>
            </div>
          </div>
          <div class="food-info" v-show="food.info">
            <split></split>
            <h3 class="product-desciption">商品介绍</h3>
            <p class="food-info-txt">{{food.info}}</p>
          </div>
          <div class="food-evaluate" v-show="food.ratings">
            <split></split>
            <h3 class="evaluate-name">商品评价</h3>
            <evaluate @ratingsType="ratingsType" @onlyContent-fun="onlyContent_fun" :ratings="food.ratings" :desc="desc" :selectType="selectType" :onlyContent="onlyContent"></evaluate>
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
  import split from 'components/split/split';
  import evaluate from 'components/evaluate/evaluate';
  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;
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
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          satisfaction: '推荐',
          displeasure: '吐槽'
        }
      };
    },
    methods: {
      ratingsType (type) {
        this.selectType = type;
      },
      onlyContent_fun () {
        this.onlyContent = !this.onlyContent;
      },
      detailShow () {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.detailScroll) {
            this._detailScroll();
          } else {
            this.detailScroll.refresh();
          };
        });
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
        };
        this.$emit('cartadd', event.target);
      }
    },
    components: {
      cartcontrol,
      split,
      evaluate
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
            width: 86px
            height: 24px
            font-size: 0
            .addCart
              position: absolute
              right: 0
              top: 0
              display: inline-block
              width: 60px
              height: 24px
              background: rgb(0, 160, 220)
              border-radius: 12px
              font-size: 10px
              line-height: 24px
              padding: 0 12px
              color: #fff
              text-align: center
              transition: all 0.2s linear
              opacity: 1
              &.fade-enter, &.fade-leave-active
                opacity: 0
            .cartcontrol-wrapper
              position: absolute
              right: 0
              top: 0
              width: 68px
        .food-info
          font-size: 0
          background: #fff
          .product-desciption
            padding: 18px 18px 6px 18px
            font-size: 14px
            line-height: 14px
            color: rgb(7, 17, 27)
          .food-info-txt
            padding: 0 26px 18px 26px
            line-height: 24px
            font-weight: 200
            font-size: 12px
            color: rgb(77, 85, 93)
        .food-evaluate
          background: #fff
          .evaluate-name
            padding: 18px 18px 6px 18px
            font-size: 14px
            line-height: 14px
            color: rgb(7, 17, 27)
</style>
