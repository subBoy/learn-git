<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li class="menu-item" v-for="(item, index) in goods">
          <span class="text">
            <span class="icon" :class="classMap[item.type]" v-show="item.type > 0"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li class="food-list" v-for="(item, index) in goods">
          <h1 class="title" v-text="item.name"></h1>
          <ul>
            <li class="food-item" v-for="(food, index) in item.foods">
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
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: []
      };
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
        }
      });
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
        margin: 0 auto
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
          color: rgb(240, 20, 20)
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
          .icon
            width: 68px
            .icon-img
              width: 58px
          .content
            flex: 1
            .name
              font-size: 14px
              line-height: 14px
              color: rgb(7, 17, 27)
              padding: 2px 0 8px 0
            .desc
              font-size: 10px
              line-height: 10px
              color: rgb(147, 153, 159)
              padding-bottom: 8px
            .sellCount
              display: inline-block
              font-size: 10px
              line-height: 10px
              color: rgb(147, 153, 159)
              padding-right: 12px
            .rating
              display: inline-block
              font-size: 10px
              line-height: 10px
              color: rgb(147, 153, 159)
            .price
              font-size: 10px/14px
              color: rgb(240, 20, 20)
              font-weight: normal/700
              line-height: 24px
              font-family: "微软雅黑"
            .oldPrice
              font-size: 10px
              color: rgb(147, 153, 159)
              font-weight: normal/700
              line-height: 24px
              font-family: "微软雅黑"
</style>

