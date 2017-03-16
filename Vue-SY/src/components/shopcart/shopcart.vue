<template>
  <div class="shopcart-component">
    <div class="shopcart">
      <div class="content" @click="toggleList">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo" :class="{'highlight': totalCount > 0}">
              <i class="icon-shopping_cart" :class="{'highlight': totalCount > 0}"></i>
            </div>
            <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'highlight': totalPrice > 0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right">
          <div class="pay" @click.stop.prevent="pay" :class="payClass">{{payDesc}}</div>
        </div>
      </div>
      <div class="ball-container">
        <transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter" v-for="ball in balls">
          <div class="ball" v-show="ball.show">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </div>
      <transition name="fold">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="removeFoods">清空</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food" v-for="food in selectFoods">
                <span class="name">{{food.name}}</span>
                <div class="price"><span>￥{{food.price * food.count}}</span></div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol @cartadd="drop" :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <transition name="fade">
      <div class="list-mask" @click="hideList" v-show="listShow"></div>
    </transition>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  export default {
    props: {
      selectFoods: {
        type: Array,
        default () {
          return [];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data () {
      return {
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: [],
        fold: true
      };
    },
    computed: {
      totalPrice () {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount () {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}起送`;
        } else {
          return '去结算';
        }
      },
      payClass () {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true;
          return false;
        };
        let show = !this.fold;
        if (show) {
          this.$nextTick(() => {
            if (!this.listScroll) {
              this._initScroll();
            } else {
              this.listScroll.refresh();
            };
          });
        };
        return show;
      }
    },
    methods: {
      pay () {
        if (this.totalPrice < this.minPrice) {
          return;
        };
        window.alert('支付' + this.totalPrice);
      },
      hideList () {
        this.fold = true;
      },
      removeFoods () {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      _initScroll () {
        this.listScroll = new BScroll(this.$refs.listContent, {
          click: true
        });
      },
      toggleList () {
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      drop (el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        };
      },
      beforeEnter (el) {
        let count = this.balls.length;
        while (count--) {
          let ball = this.balls[count];
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect();
            let x = rect.left - 32;
            let y = -(window.innerHeight - rect.top - 22);
            el.style.display = '';
            el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
            el.style.transform = `translate3d(0, ${y}px, 0)`;
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
            inner.style.transform = `translate3d(${x}px, 0, 0)`;
          };
        }
      },
      enter (el) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight;
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0, 0, 0)';
          el.style.transform = 'translate3d(0, 0, 0)';
          let inner = el.getElementsByClassName('inner-hook')[0];
          inner.style.webkitTransform = 'translate3d(0, 0, 0)';
          inner.style.transform = 'translate3d(0, 0, 0)';
        });
      },
      afterEnter (el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
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
  .shopcart-component
    .shopcart
      position: fixed
      left: 0
      bottom: 0
      z-index: 50
      width: 100%
      height: 48px
      .content
        display: flex
        background: #141d27
        height: 48px
        font-size: 0
        .content-left
          flex: 1
          .logo-wrapper
            display: inline-block
            vertical-align: top
            position: relative
            top: -10px
            padding: 6px
            margin: 0 12px
            width: 56px
            height: 56px
            box-sizing: border-box
            border-radius: 100%
            background: #141d27
            .logo
              width: 100%
              height: 100%
              border-radius: 100%
              background: #2b343c
              text-align: center
              &.highlight
                background: rgb(0, 160, 220)
              .icon-shopping_cart
                font-size: 24px
                line-height: 44px
                color: #80828a
                &.highlight
                  color: #fff
            .num
              position: absolute
              top: 0
              right: 0
              width: 24px
              height: 16px
              line-height: 16px
              text-align: center
              border-radius: 16px
              font-size: 9px
              font-weight: 700
              color: #fff
              background:rgb(240, 20, 20)
              box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
          .price
            display: inline-block
            vertical-align: top
            line-height: 24px
            margin-top: 12px
            padding-right: 12px
            box-sizing: border-box
            border-right: 1px solid rgba(255, 255, 255, 0.1)
            font-size: 16px
            font-weight: 700
            color: rgba(255, 255, 255, 0.4)
            &.highlight
              color: #fff
          .desc
            display: inline-block
            vertical-align: top
            margin: 12px 0 0 12px
            line-height: 24px
            font-size: 10px
            color: rgba(255, 255, 255, 0.4)
        .content-right
          flex: 0 0 105px
          width: 105px
          .pay
            height: 48px
            line-height: 48px
            padding: 0px 8px
            text-align: center
            background: #2b333b
            box-sizing: border-box
            font-size: 12px
            font-weight: 700
            color: rgba(255, 255, 255, 0.4)
            &.not-enough
              background: #2b333b
            &.enough
              background: #00b43c
              color: #fff
      .ball-container
        .ball
          position: fixed
          left: 32px
          bottom: 22px
          z-index: 200
          transition: all .4s cubic-bezier(.36,-0.36,.83,.67)
          .inner
            width: 16px
            height: 16px
            border-radius: 100%
            background: rgb(0, 160, 220)
            transition: all .4s linear
      .shopcart-list
        position: absolute
        top: 0
        left: 0
        z-index: -1
        width: 100%
        transition: all .5s
        transform: translate3d(0, -100%, 0)
        &.fold-enter, &.fold-leave-active
          transform: translate3d(0, 0, 0)
        .list-header
          display: flex
          hieght: 40px
          line-height: 40px
          padding: 0 18px
          background: #f3f5f7
          border-bottom: 1px solid rgba(7, 17, 27, 0.1)
          .title
            flex: 1
            font-weight: 14px
            font-weight: 200
            color: rgb(7, 17, 27)
          .empty
            display: inline-block
            flex: 0 0 24px
            font-size: 12px
            color: rgb(0, 160, 220)
            width: 24px
        .list-content
          padding: 0 18px
          background: #fff
          max-height: 217px
          overflow: hidden
          .food
            display: flex
            padding: 12px 0
            border-1px(rgba(7, 17, 27, 0.1))
            &:last-child
              border-none()
            .name
              flex: 1
              height: 24px
              line-height: 24px
              font-weight: 700
            .price
              height: 24px
              line-height: 24px
              padding: 0 12px 0 18px
              font-size: 14px
              font-weight: 700
              color: rgb(240, 20, 20)
              font-family: "微软雅黑"
    .list-mask
      position: fixed
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: 40
      backdrop-filter: blur(10px)
      transition: all .5s
      opacity: 1
      background: rgba(7, 17, 27, 0.6)
      &.fade-enter, &.fade-leave-active
        opacity: 0
        background: rgba(7, 17, 27, 0)
</style>
