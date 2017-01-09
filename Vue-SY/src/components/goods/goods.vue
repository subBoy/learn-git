<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li class="menu-item menu-item-hook" :class="{'current':currentIndex===index}" @click="selectMenu(index, $event)" v-for="(item, index) in goods">
          <span class="text">
            <span class="icon" :class="classMap[item.type]" v-show="item.type > 0"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list food-list-hook" v-for="item in goods">
          <h1 class="title" v-text="item.name"></h1>
          <ul>
            <li class="food-item border-1px" v-for="food in item.foods" @click="selectFood(food, $event)">
              <div class="icon">
                <img class="icon-img" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h3 class="name" v-text="food.name"></h3>
                <p class="desc" v-text="food.description"></p>
                <div class="extra">
                  <span class="sellCount">月售：{{food.sellCount}} 份</span>
                  <span class="rating">好评率：{{food.rating}}%</span>
                </div>
                <div class="pricees">
                  <span class="price">￥{{food.price}}</span>
                  <span class="oldPrice" v-if="food.oldPrice != ''">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol @cartadd="getCartadd" :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    <detail ref="detail" :cartadd="getCartadd" @cartadd="getCartadd" :food="selectedFood"></detail>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import detail from 'components/detail/detail';

  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            let itemList = this.$refs.menuWrapper.getElementsByClassName('menu-item-hook');
            let el = itemList[i];
            this.meunScroll.scrollToElement(el, 300);
            return i;
          }
        };
        return 0;
      },
      selectFoods () {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          let _this = this;
          this.$nextTick(function () {
            _this._initScroll();
            _this._calculateHeight();
          });
        }
      });
    },
    methods: {
      selectFood (food, event) {
        if (!event._constructed) {
          return;
        };
        this.selectedFood = food;
        this.$refs.detail.detailShow();
      },
      selectMenu (index, event) {
        if (!event._constructed) {
          return;
        };
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 300);
      },
      _drop (target) {
        // 体验优化， 执行下落动画
        this.$nextTick(() => {
          this.$refs.shopcart.drop(target);
        });
      },
      getCartadd (target) {
        this._drop(target);
      },
      _initScroll (event) {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        let _this = this;
        this.foodScroll.on('scroll', function (pos) {
          _this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight () {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        };
      }
    },
    components: {
      shopcart,
      cartcontrol,
      detail
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        width: 56px
        height: 54px
        padding: 0 12px
        &.current
          position: relative
          z-index: 10
          margin-top: -1px
          background: #fff
          .text
            border-none()
            color: rgb(240, 20, 20)
            font-weight: 700
        .icon
          display: inline-block
          vertical-align: top
          height: 12px
          width: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-img("decrease_3")
          &.discount
            bg-img("discount_3")
          &.guarantee
            bg-img("guarantee_3")
          &.invoice
            bg-img("invoice_3")
          &.special
            bg-img('special_3')
        .text
          display: table-cell
          width: 56px
          vertical-align: middle
          font-size: 12px
          color: #07111b
          font-weight: 200
          line-height: 14px
          border-1px(rgba(7, 17, 27, 0.1))
    .foods-wrapper
      flex: 1
      .food-list
        // font-size: 0
        .title
          height: 26px
          line-height: 26px
          background: #f3f5f7
          border-left: 2px solid #d9dde1
          padding-left: 18px
          font-size: 12px
          color: rgb(147, 153, 159)
        .food-item
          display: flex
          margin: 0 18px
          padding: 18px 0
          border-1px(rgba(7, 17, 27, 0.1))
          &:last-child
            border-none()
          .icon
            flex: 0 0 68px
            width: 68px
            .icon-img
              width: 58px
          .content
            flex: 1
            .name
              font-size: 14px
              line-height: 14px
              height: 14px
              color: rgb(7, 17, 27)
              margin: 2px 0 8px 0
            .desc
              font-size: 10px
              line-height: 10px
              color: rgb(147, 153, 159)
              padding-bottom: 8px
            .extra
              font-size: 0
              .sellCount
                display: inline-block
                font-size: 10px
                line-height: 10px
                color: rgb(147, 153, 159)
                padding: 0 12px 8px 0
              .rating
                display: inline-block
                font-size: 10px
                line-height: 10px
                color: rgb(147, 153, 159)
                padding: 0 0 8px 0
            .pricees
              font-size: 0
              .price
                font-size: 14px
                color: rgb(240, 20, 20)
                font-family: "微软雅黑"
                padding-right: 6px
              .oldPrice
                font-size: 10px
                color: rgb(147, 153, 159)
                font-weight: 700
                line-height: 24px
                font-family: "微软雅黑"
                text-decoration:line-through
            .cartcontrol-wrapper
              position: absolute
              right: 0
              bottom: 12px
</style>

