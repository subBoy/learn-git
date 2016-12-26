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
                <img :src="food.icon" alt="">
              </div>
              <div class="content">
                <h3 class="name" v-text="food.name"></h3>
                <p class="desc" v-text="food.description"></p>
                <div class="extra">
                  <span>月售：{{food.sellCount}} 份</span>
                  <span>好评率：{{food.rating}}%</span>
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
</style>

